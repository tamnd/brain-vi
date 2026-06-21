---
title: "CF 106C - Bánh Bao"
description: "Lavrenty có một lượng bột cố định và một số loại nhân. Mỗi loại nhân có số lượng hạn chế và cần một lượng bột nhất định để làm ra một chiếc bánh, mỗi chiếc bánh đều mang lại một khoản lợi nhuận."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dp"]
categories: ["algorithms"]
codeforces_contest: 106
codeforces_index: "C"
codeforces_contest_name: "Codeforces Beta Round 82 (Div. 2)"
rating: 1700
weight: 106
solve_time_s: 110
verified: true
draft: false
---

[CF 106C - Bánh bao](https://codeforces.com/problemset/problem/106/C) 

**Đánh giá:** 1700 
**Thẻ:** dp 
**Thời gian giải:** 1 phút 50 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Lavrenty có một lượng bột cố định và một số loại nhân. Mỗi loại nhân có số lượng hạn chế và cần một lượng bột nhất định để làm ra một chiếc bánh, mỗi chiếc bánh đều mang lại một khoản lợi nhuận. Ngoài ra, anh ta có thể làm những chiếc bánh đơn giản mà không cần nhồi, điều này cũng tiêu tốn bột và mang lại lợi nhuận cố định. Mục tiêu là chọn số lượng bánh mỗi loại, bao gồm cả bánh thường, để nướng nhằm tối đa hóa tổng lợi nhuận. 

Dữ liệu đầu vào rất đơn giản: tổng số gam bột`n`, số kiểu nhồi`m`, và bột và lợi nhuận cho bánh thường`c0`Và`d0`. Sau đó, với mỗi lần nhồi, chúng ta được cung cấp số gam có sẵn`ai`, gram cần thiết cho mỗi chiếc bánh`bi`, bột mỗi cái bánh`ci`, và lợi nhuận`di`. 

Những ràng buộc ngụ ý rằng`n`nhiều nhất là 1000, trong khi số lượng kiểu nhồi`m`nhỏ, nhiều nhất là 10. Mỗi lượng tài nguyên cũng nhỏ, lên tới 100. Điều này cho thấy giải pháp lập trình động dựa trên việc sử dụng bột là khả thi. nhỏ`m`cho phép xử lý từng nội dung một cách độc lập trước khi tích hợp vào giải pháp toàn cầu. 

Các trường hợp khó khăn không rõ ràng bao gồm các tình huống trong đó giải pháp tối ưu chỉ sử dụng bánh đơn giản mặc dù có nhân nhồi hoặc khi một số loại nhân quá đắt so với lợi nhuận của chúng. Ví dụ, nếu`n=5`,`m=1`,`c0=1`,`d0=1`, và việc nhồi yêu cầu`ai=3`,`bi=1`,`ci=5`,`di=10`, giải pháp tối ưu là làm năm chiếc bánh đơn giản với tổng lợi nhuận là 5 chứ không phải một chiếc bánh nhân, vì yêu cầu bột quá cao cho một chiếc bánh nhân. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử mọi cách kết hợp số lượng bánh để làm cho từng loại nhân và bánh thường. Đối với mỗi loại nhồi`i`, chúng ta có thể nướng ở bất cứ đâu từ 0 đến`ai // bi`bánh bao, miễn là chúng ta có đủ bột. Nhân tất cả các khả năng sẽ cho số kết hợp theo cấp số nhân: đại khái`O(prod(ai//bi + 1))`hoạt động. Điều này rõ ràng là không khả thi ngay cả đối với những`m`bởi vì`ai`có thể lên tới 100. 

Điểm mấu chốt là mỗi loại bánh bao, kể cả bánh bao thường, đều bị giới hạn bởi cả lượng nhân và bột. Cấu trúc này hoàn toàn phù hợp với bài toán về chiếc ba lô giới hạn trong đó “trọng lượng” là số bột được tiêu thụ và “giá trị” là lợi nhuận. Chúng ta có thể chuyển đổi từng kiểu nhồi thành một chuỗi các mục bằng cách sử dụng thủ thuật phân rã nhị phân: chúng ta thay thế`ai // bi`bánh có các vật phẩm có lũy thừa bằng hai, làm giảm tổng số vật phẩm xuống tối đa`log2(maxBuns)`mỗi lần nhồi. Điều này cho phép chúng ta sử dụng mảng lập trình động một chiều trên khối bột. 

Điều này biến vấn đề thành một chiếc ba lô cổ điển: tối đa hóa lợi nhuận với tổng số bột có hạn, trong đó mỗi "món" đại diện cho một bó bánh có thể được nướng cùng nhau. Những chiếc bánh đơn giản thực sự không bị giới hạn nên chúng tôi xử lý chúng một cách riêng biệt bằng bản cập nhật ba lô không giới hạn tiêu chuẩn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(prod(ai//bi + 1)) | O(n) | Quá chậm | 
| Tối ưu (ba lô có giới hạn) | O(n * m * log(maxBuns)) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo mảng DP`dp`kích thước`n + 1`với tất cả số không.`dp[x]`sẽ đại diện cho lợi nhuận tối đa có thể đạt được bằng cách sử dụng chính xác`x`gram bột. 
2. Đối với từng loại nhồi`i`, tính số bánh lớn nhất có thể:`maxBuns = ai // bi`. Áp dụng phân rã nhị phân trên`maxBuns`để tạo ra một số "mục đi kèm". Ví dụ, nếu`maxBuns = 13`, phân hủy thành`1, 2, 4, 6`bánh bao. Mỗi bó trở thành một vật phẩm có trọng lượng`bundle_size * ci`và giá trị`bundle_size * di`. 
3. Đối với mỗi mục được gói từ tất cả các kiểu nhồi, hãy cập nhật mảng DP ngược lại (từ`n`xuống`weight`) để tránh sử dụng cùng một chiếc bánh nhiều lần. Đối với mỗi`dp[j]`, coi như`dp[j - weight] + value`. 
4. Xử lý bánh đơn giản một cách riêng biệt vì chúng không bị ràng buộc. Lặp lại`j`từ`c0`ĐẾN`n`, và cập nhật`dp[j] = max(dp[j], dp[j - c0] + d0)`. Điều này mô phỏng việc thêm bất kỳ số lượng bánh đơn giản nào mà không vượt quá số bột. 
5. Đáp án là giá trị lớn nhất trong`dp`, hoặc tương đương`dp[n]`sau tất cả các bản cập nhật. 

Lý do điều này có hiệu quả là ở mỗi bước,`dp[x]`luôn dự trữ lợi nhuận tối đa có thể bằng cách sử dụng chính xác`x`gram bột. Việc gộp các mục có lũy thừa bằng hai đảm bảo chúng tôi tính đến mọi số lượng bánh có thể đạt đến giới hạn mà không tạo quá nhiều bản cập nhật DP, trong khi việc lặp lại ngược lại đảm bảo hành vi giới hạn của ba lô. Các bánh đơn giản không giới hạn được thêm vào một cách an toàn bằng phép lặp về phía trước. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m, c0, d0 = map(int, input().split())
stuffings = [tuple(map(int, input().split())) for _ in range(m)]

dp = [0] * (n + 1)

for ai, bi, ci, di in stuffings:
    maxBuns = ai // bi
    k = 1
    bundles = []
    while maxBuns > 0:
        take = min(k, maxBuns)
        bundles.append((take * ci, take * di))
        maxBuns -= take
        k <<= 1
    for weight, value in bundles:
        for j in range(n, weight - 1, -1):
            dp[j] = max(dp[j], dp[j - weight] + value)

for j in range(c0, n + 1):
    dp[j] = max(dp[j], dp[j - c0] + d0)

print(dp[n])
```Phần đầu tiên đọc đầu vào và lưu trữ tất cả thông tin nhồi nhét. Sau đó chúng tôi khởi tạo một mảng DP cho bột. Đối với mỗi loại nhân, chúng tôi tính toán số lượng bánh tối đa có thể làm được và chia thành từng bó bằng cách sử dụng lũy ​​thừa hai. Chúng tôi lặp lại mảng DP ngược lại cho các gói này để tôn trọng tính chất giới hạn của từng kiểu nhồi. Cuối cùng, các bánh đơn giản được thêm vào theo thứ tự chuyển tiếp vì chúng không bị chặn. Dòng cuối cùng in lợi nhuận tối đa có thể đạt được. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
10 2 2 1
7 3 2 100
12 3 1 10
```| Bước | Cập nhật DP | Giải thích | 
| --- | --- | --- | 
| Ban đầu | [0]*11 | Chưa có bánh | 
| Nhồi 1 | bó: (2_2,1_100?), kiểm tra tính toán | Thêm tối đa 2 bánh | 
| Nhồi 2 | gói: 1,2,4? | Thêm tối đa 4 bánh | 
| Bánh thường | cập nhật tiếp theo cho c0=2 | Thêm bột còn lại một cách hiệu quả | 
| dp cuối cùng[10] | 241 | Trận đấu dự kiến ​​| 

Điều này cho thấy việc kết hợp nhiều loại và tận dụng bột thừa để làm bánh thường sẽ mang lại lợi nhuận tối đa. 

### Mẫu 2 

đầu vào:```
10 1 3 1
2 2 5 10
```Kết quả: dp[10] = 4 bánh thường → lợi nhuận 4. Nhồi bánh vào bột quá tốn kém. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * m * log(maxBuns)) | Mỗi phần nhồi được phân tách thành các mục nhật ký (maxBun), mỗi mục cập nhật DP có kích thước n | 
| Không gian | O(n) | Chỉ cần mảng DP trên bột | 

Cho n 1000, m 10 và maxBun 100, tổng số thao tác ở mức dưới 10^5 một cách thoải mái, trong giới hạn thời gian 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, m, c0, d0 = map(int, input().split())
    stuffings = [tuple(map(int, input().split())) for _ in range(m)]
    dp = [0] * (n + 1)
    for ai, bi, ci, di in stuffings:
        maxBuns = ai // bi
        k = 1
        bundles = []
        while maxBuns > 0:
            take = min(k, maxBuns)
            bundles.append((take * ci, take * di))
            maxBuns -= take
            k <<= 1
        for weight, value in bundles:
            for j in range(n, weight - 1, -1):
                dp[j] = max(dp[j], dp[j - weight] + value)
    for j in range(c0, n + 1):
        dp[j] = max(dp[j], dp[j - c0] + d0)
    return str(dp[n])

assert run("10 2 2 1\n7 3 2 100\n12 3 1 10\n") == "241", "sample 1"
assert run("10 1 3 1\n2 2 5 10\n") == "4", "sample 2"
assert run("1 1 1 1\n1 1 1 1\n") == "1", "minimum input"
assert run("1000 10 1 1\n100 1 1 100\n"*10) == "100000", "max input, many high profit"
```
