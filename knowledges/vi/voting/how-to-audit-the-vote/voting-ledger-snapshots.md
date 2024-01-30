---
title: Ảnh chụp sổ cái biểu quyết
date: '2020-10-06 08:48:23 +0000'
lastmod: '2020-10-06 08:48:23 +0000'
draft: 'false'
images: []
weight: '10'
---

With snapshots of the voting ledger published hourly, anyone can verify that recorded votes remain unchanged across the voting lifecycle.

The Catalyst team provides access to routine snapshots of the blockchain state. These snapshots are automatically taken at 1-hour intervals and uploaded to a persistent storage system in the form of an archive. Currently, these archives can be found by [querying the Archive API](https://archiver.projectcatalyst.io/api/v1/archives/) . You can learn more about the available endpoints by inspecting the [Swagger documentation](https://archiver.projectcatalyst.io/swagger/index.html)

To show an example of how to interact with the API and download an archive, we will use the curl and jq CLI tools. First, we must list the available archives:

```json

> curl -s https://archiver.projectcatalyst.io/api/v1/archives/ | jq
{
  "next": "",
  "archives": [
    {
      "id": "019fd6b2-c36b-419f-8a2d-ebf06b91a762",
      "size": 364315029,
      "compression": "zstd",
      "packaging": "tar",
      "metadata": {
        "epoch": "1952",
        "fund": "fund10",
        "slot": "14",
        "timestamp": "2023-08-29T18:00:59Z"
      }
    },
    {
      "id": "64c56031-56ab-43dd-a1ca-138a85298d2b",
      "size": 372405154,
      "compression": "zstd",
      "packaging": "tar",
      "metadata": {
        "epoch": "1954",
        "fund": "fund10",
        "slot": "17",
        "timestamp": "2023-08-29T20:01:09Z"
      }
    },
    {
      "id": "ad16b160-0cc2-4252-ae61-f468c95cbf29",
      "size": 364453534,
      "compression": "zstd",
      "packaging": "tar",
      "metadata": {
        "epoch": "1953",
        "fund": "fund10",
        "slot": "23",
        "timestamp": "2023-08-29T19:01:34Z"
      }
    }
  ]
}

```

Trong phản hồi ở trên, chỉ có ba kho lưu trữ có sẵn. Nếu số lượng kết quả vượt quá 50, chúng sẽ tự động được phân trang. Điều này được thể hiện rõ qua trường tiếp theo được đặt. Để tìm nạp các bản lưu trữ tiếp theo, bạn sẽ gọi cùng một điểm cuối và nối thêm ?next={VALUE} trong đó VALUE là giá trị của trường tiếp theo. Để lặp lại tất cả các kho lưu trữ có sẵn, bạn có thể lặp lại quy trình này cho đến khi trường tiếp theo trống.

For this example, we'll choose to fetch the first archive in the list. To get a link for downloading the archive, we can run:

```json

> curl -q https://archiver.projectcatalyst.io/api/v1/archives/019fd6b2-c36b-419f-8a2d-ebf06b91a762/download | jq
{
  "url": "https://archives.projectcatalyst.io/prod/019fd6b2-c36b-419f-8a2d-ebf06b91a762?Expires=1693341938&Signature=R5CJdg4GZCHHGakePZJIHaYHSOthO-RuIAuiGwcLTnD3MZrtxUMWQFvLdSpWrl6dqPB6VNNeS5sMp9pK7x-JmuBvnZ3XZNUEcBA9XLlMgIZJQDD7l6JEgCtKWRiOFPbOSUZSLQMhD6mbL2koARzdZjkzLDjPFFf33~vU89qZzt-VaaMseDUtGv-6zU6ANh2RkUvWD9UCUDTwoU9VjrMhwPfrx2kaWGIkt5a3NqxkNmti7SVdwtcsKWN7wuLQNaks-PJnrTKwtp7Qc8Ll3vrf846vJWzH3UVDwDB0vbk1nVcysijEaj6m7DcWA5TR7Di84FHjYf9zmTJYeeC71Ht8mw__&Key-Pair-Id=K36UOCCH06A5FV"
}

```

Điểm cuối này trả về một URL có giới hạn thời gian để tải xuống kho lưu trữ. Lưu ý rằng URL này chỉ có hiệu lực trong 15 phút. Để tải xuống kho lưu trữ:

```

> URL="$(curl -s https://archiver.projectcatalyst.io/api/v1/archives/019fd6b2-c36b-419f-8a2d-ebf06b91a762/download | jq -r .url)"
> curl -o archive.tar.zstd "$URL"

```

We save the file as a `.tar` `.zstd` archive because this is how the contents were archived. If you look back at the API response, you'll see the compression and packaging fields were set to `zstd` and tar respectively. We can use these fields to determine the process for extracting the archive. To extract the contents, we'll need to use the `.tar` and `.zstd` CLIs:

```

> zstd -d archive.tar.zstd && tar -xvf archive.tar
archive.tar.zstd    : 523103232 bytes
artifacts
artifacts/block0.bin
permanent
permanent/blocks
permanent/blocks/data
permanent/blocks/seqno
permanent/chain_length
permanent/chain_length/data
permanent/chain_length/seqno
volatile
volatile/blobs
volatile/conf
volatile/db
volatile/snap.00000003A7319A1C

```

We now have a complete snapshot of the blockchain state on our local filesystem.
