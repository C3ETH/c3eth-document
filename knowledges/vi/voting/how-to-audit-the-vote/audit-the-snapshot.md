---
title: Kiểm tra Snapshot
date: '2020-10-06 08:48:23 +0000'
lastmod: '2020-10-06 08:48:23 +0000'
draft: 'false'
images: []
weight: '30'
---

## Parameters

Registration Deadline: Jan 15, 2024 21:45:00 UTC

Minimum Voting Power:  450 ADA.

## Công cụ bắt buộc

### dbSync

Installation instructions can be found [here](https://github.com/IntersectMBO/cardano-db-sync/blob/master/doc/docker.md).

Ensure dbSync and the PostgreSQL database are running, and the database is synchronized past 21:45:00 UTC on the 20th of January 2024.

Lưu ý: *NẾU dbSync của bạn không được đồng bộ hóa ít nhất vào ngày này, kết quả từ việc chạy snapshot sẽ không chính xác. Điều này là do snapshot được chụp vào thời điểm khi việc khôi phục trên blockchain không thể ảnh hưởng đến dữ liệu snapshot và do đó nó có thể tái tạo hoàn toàn và ổn định. Điều này được đảm bảo bằng cách đặt một Kỷ nguyên đầy đủ trong khoảng thời gian từ thời hạn đăng ký đến thời điểm snapshot cuối cùng sớm nhất có thể.*

### Công cụ Catalyst snapshot

Công cụ snapshot có ở [đây](https://github.com/input-output-hk/catalyst-core) . Sao chép repo và sau đó xây dựng các công cụ cần thiết.

### Building the tools

##### snapshot_tool:

```

 cargo build -r -p voting_tools_rs

```

##### catalyst-toolbox

```

cargo build -r -p catalyst-toolbox

```

## Tái tạo snapshot

Kiểm tra snapshot chỉ đơn giản là sao chép snapshot trực tiếp từ dữ liệu blockchain và so sánh nó với dữ liệu snapshot được công bố chính thức.

### Lấy số cho Thời hạn đăng ký

Getting the `slot_no` aligned with the registration deadline.

Query the dbSync database with:

```

select slot_no, time from block
    where slot_no is not null and time <= '2024-01-15 21:45:00'
    order by slot_no desc limit 1 ;

```

The result will be:

```

113788796,2024-01-15 21:44:30.000000

```

Do đó, `slot#` mà snapshot cần nhắm mục tiêu là: 113788796

### Chạy snapshot thô

Phần đầu tiên của snapshot tích lũy thông tin đăng ký thô và thông tin đặt cọc và xác thực nó theo [CIP-15](https://cips.cardano.org/cip/CIP-15/) và [CIP-36](https://cips.cardano.org/cip/CIP-36/)

Note: *Multiple delegations, as specified by CIP-36, are not supported.  These will be detected as invalid registrations.  Only registrations that contain a single voting key are supported and valid.*

Run (replace your credentials as appropriate):

```

export USERNAME=<Your dbSync postgresql Username>
export PASSWORD=<Your dbSync postgresql Password>
export DBSYNC_POSTGRES="localhost:5432"
./target/release/snapshot_tool --db cexplorer --db-user $USERNAME --db-pass $password --db-host $DBSYNC_POSTGRES --out-file ./cexplorer-113788796.json --min-slot 0 --max-slot 113788796 --network-id mainnet

```

This will produce three files:

- `cexplorer-113788796.json` &lt;- Dữ liệu snapshot thô
- `cexplorer-113788796.unregistered.json` &lt;- Unregistered but staked ADA.
- `cexplorer-113788796.errors.json` &lt;- Errors or Obsolete registrations.

### Processing the snapshot with Fund 11 parameters

This filters registrations for minimum allowed voting power:

Run:

```

./target/release/catalyst-toolbox snapshot --snapshot cexplorer-113788796.json --min-stake-threshold 450000000 --slot-no 113788796 cexplorer-113788796.final.json

```

Điều này tạo ra snapshot cuối cùng: `cexplorer-113788796.summary.json`
