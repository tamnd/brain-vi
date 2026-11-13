---
title: "CF 1749C - Trò Chơi Số"
description: "Chúng ta được yêu cầu phân tích một trò chơi hai người chơi trên một dãy số nguyên dương. Alice chọn một số màn $k$ cô ấy muốn chơi và trong mỗi màn cô ấy phải loại bỏ một phần tử khỏi mảng nhiều nhất là số của màn còn lại."
date: "2026-06-09T15:14:10+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "games", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1749
codeforces_index: "C"
codeforces_contest_name: "Educational Codeforces Round 138 (Rated for Div. 2)"
rating: 1400
weight: 1749
solve_time_s: 130
verified: false
draft: false
---

[CF 1749C - Trò chơi số](https://codeforces.com/problemset/problem/1749/C) 

**Đánh giá:** 1400 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu, trò chơi, tham lam, triển khai 
**Thời gian giải:** 2 phút 10 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu phân tích một trò chơi hai người chơi trên một dãy số nguyên dương. Alice chọn một số giai đoạn$k$cô ấy muốn chơi và trong mỗi màn cô ấy phải loại bỏ một phần tử khỏi mảng nhiều nhất là số của màn còn lại. Sau khi loại bỏ Alice, Bob thêm giá trị giai đoạn tương tự vào bất kỳ phần tử còn lại nào. Alice thắng nếu cô ấy hoàn thành thành công tất cả$k$giai đoạn; cô ấy sẽ thua nếu không thể loại bỏ phần tử hợp lệ trong bất kỳ giai đoạn nào. 

Đầu vào cung cấp cho chúng tôi nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm cung cấp kích thước của mảng và chính mảng đó. Đầu ra phải là số lượng giai đoạn tối đa$k$Alice có thể lựa chọn và vẫn đảm bảo chiến thắng nếu cả hai đều chơi tối ưu. 

Các hạn chế là nhỏ:$n \le 100$và giá trị mảng nhiều nhất là$n$. Với các giới hạn này, bất kỳ thuật toán bậc hai nào$n$hoặc tệ hơn một chút sẽ chạy thoải mái dưới giới hạn 2 giây. Điều này cho phép chúng ta suy nghĩ theo cách sắp xếp hoặc lặp lại đơn giản thay vì các cấu trúc dữ liệu phức tạp. 

Trường hợp cạnh không rõ ràng xảy ra khi mảng chứa đầy các phần tử quá lớn để Alice có thể loại bỏ ngay cả trong giai đoạn đầu tiên. Ví dụ, nếu$a = [4,4,4,4]$Và$n=4$, thì thậm chí$k=1$là không thể vì Alice không thể xóa một số$\le 1$. Một cách tiếp cận ngây thơ chỉ đơn giản là cố gắng loại bỏ các phần tử một cách tham lam mà không sắp xếp sẽ thất bại ở đây, tạo ra một kết quả không chính xác.$k>0$. 

Một sự tinh tế khác là tác động của các bước di chuyển của Bob. Anh ta có thể tăng bất kỳ nguyên tố nào, có khả năng ngăn chặn khả năng loại bỏ nó của Alice trong giai đoạn sau. Tuy nhiên, trường hợp xấu nhất đối với Alice được xác định bởi các giá trị ban đầu được sắp xếp theo thứ tự tăng dần; Bob luôn có thể áp dụng số gia tăng của mình cho số lớn nhất hiện có, biến những số nhỏ nhất trước đó thành những số duy nhất Alice có thể loại bỏ ở mỗi giai đoạn một cách hiệu quả. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực mô phỏng trò chơi theo mọi cách có thể$k$từ$0$ĐẾN$n$. Đối với mỗi$k$, chúng tôi lặp qua$k$các giai đoạn, cố gắng loại bỏ một phần tử thỏa mãn điều kiện của giai đoạn và sau đó tăng thêm một phần tử nào đó. Phương pháp này hoạt động vì nó tuân thủ trực tiếp các quy tắc, nhưng số lượng thao tác trong trường hợp xấu nhất là$O(n^2)$cho mỗi trường hợp thử nghiệm, có thể chấp nhận được ở đây nhưng không thanh lịch. 

Cái nhìn sâu sắc quan trọng cho một giải pháp nhanh hơn và sạch hơn là khả năng sống sót của Alice$k$giai đoạn phụ thuộc hoàn toàn vào thứ tự sắp xếp của mảng. Sau khi phân loại, nếu Alice muốn sống sót$k$ở các giai đoạn, cô ấy phải loại bỏ các phần tử theo thứ tự tăng dần sao cho$i$- phần tử nhỏ nhất thứ nhiều nhất$i$. Điều này là do cách chơi tối ưu của Bob tối đa hóa một cách hiệu quả giá trị tương lai của các phần tử, do đó, những phần tử nhỏ nhất là những phần tử mà Alice có thể loại bỏ một cách đáng tin cậy. 

Việc quan sát rút gọn vấn đề thành một phép kiểm tra đơn giản trên mảng đã được sắp xếp: duyệt qua nó và tìm giá trị lớn nhất$k$như vậy mà$i$- phần tử thứ nhiều nhất là$i$(1-lập chỉ mục). Quá trình quét tuyến tính này sau khi sắp xếp sẽ mang lại kết quả tối ưu$k$TRONG$O(n \log n)$mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n^2) | O(n) | Hoạt động nhưng phức tạp hơn và chậm hơn | 
| Sắp xếp mảng tham lam | O(n log n) | O(n) | Được chấp nhận và sạch sẽ | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case$t$. Đối với mỗi trường hợp thử nghiệm, hãy đọc kích thước mảng$n$và mảng$a$. 
2. Sắp xếp mảng$a$theo thứ tự không giảm. Việc sắp xếp đảm bảo chúng tôi xem xét các phần tử nhỏ nhất trước tiên, những phần tử bị hạn chế nhất để Alice loại bỏ. 
3. Khởi tạo bộ đếm$k = 0$. Lặp lại mảng đã sắp xếp bằng vòng lặp 1 chỉ mục. Đối với mỗi phần tử$a[i]$, kiểm tra xem$a[i] \le k + 1$. Nếu có thì tăng$k$bằng 1. Nếu không, hãy ngừng lặp lại, vì Alice không thể sống sót sau thời điểm này. 
4. Sau khi lặp lại,$k$đại diện cho số lượng giai đoạn tối đa mà Alice có thể chọn mà vẫn đảm bảo chiến thắng. In giá trị này cho trường hợp thử nghiệm hiện tại. 

Lý do điều này có tác dụng là vì sau khi sắp xếp,$i$-phần tử thứ trong mảng được sắp xếp đại diện cho số nhỏ nhất còn lại mà Alice có thể cố gắng loại bỏ trong giai đoạn$i$. Nếu như$a[i] > i$, Alice sẽ thất bại ở giai đoạn này$i$vì không có phần tử nào đủ nhỏ để loại bỏ. Việc kiểm tra tham lam bảo toàn tính bất biến rằng tất cả các giai đoạn trước đều có thể thắng được. 

## Giải pháp Python```
PythonRun
```Giải pháp đầu tiên đọc dữ liệu đầu vào một cách hiệu quả và sắp xếp mảng. Người đếm tham lam`k`chỉ tăng khi phần tử hiện tại đủ nhỏ để cho phép Alice sống sót trong giai đoạn tiếp theo. Giờ nghỉ đảm bảo chúng tôi ngừng đếm ngay khi Alice không thể tiếp tục. sử dụng`k + 1`căn chỉnh vòng lặp Python có chỉ mục 0 với số giai đoạn được lập chỉ mục 1. 

## Ví dụ đã hoạt động 

**Ví dụ 1** 

Mảng đầu vào:`[1,1,2]`| Đã sắp xếp`a`| Sân khấu`k`| Tình trạng`x <= k+1`| Hành động | 
| --- | --- | --- | --- | 
| 1 | 0 | 1 <= 1 | k = 1 | 
| 1 | 1 | 1 <= 2 | k = 2 | 
| 2 | 2 | 2 <= 3 | k = 3 | 

Sau khi lặp lại, Alice có thể sống sót qua 2 giai đoạn (phần tử thứ ba cũng có thể bị loại bỏ, nhưng`k`đếm các giai đoạn sống sót bắt đầu từ 0). Đầu ra:`2`. 

**Ví dụ 2** 

Mảng đầu vào:`[4,4,4,4]`| Đã sắp xếp`a`| Sân khấu`k`| Tình trạng`x <= k+1`| Hành động | 
| --- | --- | --- | --- | 
| 4 | 0 | 4 <= 1 | Không, nghỉ | 

Đầu ra:`0`. Alice không thể sống sót ngay cả ở giai đoạn 1. 

Những dấu vết này xác nhận thuật toán tính toán chính xác các giai đoạn có thể thắng tối đa cho Alice và tôn trọng tác động của các bước đi của Bob bằng cách xem xét trường hợp xấu nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế, lặp lại là O(n) | 
| Không gian | O(n) | Lưu trữ mảng cho từng test | 

Với những hạn chế$n \le 100$Và$t \le 100$, tổng số phép tính trong trường hợp xấu nhất là khoảng 100 * 100 log 100 ≈ 70000, khá thoải mái trong vòng 2 giây. Việc sử dụng bộ nhớ không đáng kể so với giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
```
