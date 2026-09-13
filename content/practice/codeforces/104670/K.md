---
title: "CF 104670K - Kiến thức về nút thắt"
description: "Chúng ta được cung cấp một tập hợp nhỏ các số nhận dạng nút thắt cố định, được đánh số từ 1 đến 1000. Sonja được giao một danh sách chính xác n nút thắt riêng biệt mà cô ấy phải học."
date: "2026-06-29T09:37:13+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104670
codeforces_index: "K"
codeforces_contest_name: "2021-2022 ACM-ICPC Nordic Collegiate Programming Contest (NCPC 2021)"
rating: 0
weight: 104670
solve_time_s: 56
verified: true
draft: false
---

[CF 104670K - Kiến thức về nút thắt](https://codeforces.com/problemset/problem/104670/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp nhỏ các số nhận dạng nút cố định, được đánh số từ 1 đến 1000. Sonja được chỉ định một danh sách chính xác`n`những nút thắt riêng biệt mà cô ấy phải học. Cô ấy đã học rồi`n-1`trong số chúng, cũng được đưa ra dưới dạng danh sách, và chính xác những gì cô ấy đã học còn thiếu một nút thắt bắt buộc. 

Nhiệm vụ là xác định số còn thiếu đó, biết rằng mọi nút thắt đã học được đảm bảo là một phần của tập hợp bắt buộc ban đầu. 

Những hạn chế là rất nhỏ, với`n`lên tới 50 và giá trị lên tới 1000. Điều này ngay lập tức cho chúng ta biết rằng ngay cả những phương pháp rất đơn giản cũng đủ. Bất kỳ giải pháp nào chạy theo thời gian tuyến tính hoặc gần tuyến tính trong`n`sẽ ngay lập tức. Ngay cả việc quét bậc hai hoặc lồng nhau trên 50 phần tử vẫn không đáng kể. 

Không có ràng buộc ẩn phức tạp nào như trùng lặp hoặc thiếu nhiều giá trị. Cả hai danh sách đều được tuyên bố rõ ràng là chứa các giá trị riêng biệt, vì vậy chúng ta không cần phải xử lý các bội số hoặc sự mơ hồ về tần số. 

Trường hợp cạnh chính là cấu trúc hơn là tính toán. Vì giá trị còn thiếu được đảm bảo tồn tại và duy nhất nên mọi cách tiếp cận đều phải duy trì lý do thành viên chính xác. Một sai lầm ngây thơ sẽ là giả định thứ tự hoặc sự liên kết giữa hai danh sách. Ví dụ: nếu một người cố gắng so sánh các vị trí thay vì đặt tư cách thành viên, kết quả sẽ thất bại do thứ tự đầu vào là tùy ý. 

Trường hợp khó phát hiện thứ hai là quên rằng phần tử bị thiếu luôn nằm trong danh sách đầu tiên. Nếu ai đó tính sai sai số đối xứng hoặc so sánh với phạm vi đầy đủ từ 1 đến 1000, họ có thể vô tình chọn một số không liên quan đến ràng buộc đầu vào. 

Ví dụ trong đó so sánh vị trí ngây thơ không thành công: 

đầu vào:```
4
1 2 3 4
4 2 3
```Đầu ra đúng:```
1
```Nếu một người trừ sai bằng cách căn chỉnh chỉ mục (chỉ so sánh các phần tử đầu tiên), họ sẽ nghĩ nhầm`1`vs`4`không khớp ngụ ý một cái gì đó không liên quan. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: với mỗi nút thắt trong danh sách bắt buộc, hãy quét qua danh sách đã học và kiểm tra xem nó có xuất hiện hay không. Nếu chúng ta tìm thấy nút thắt bắt buộc không xuất hiện trong danh sách đã học thì đó chính là câu trả lời. 

Điều này có tác dụng vì tư cách thành viên là điều kiện duy nhất xác định tính đúng đắn. Tuy nhiên, đối với mỗi`n`các nút thắt cần thiết, quét danh sách chi phí đã học`O(n)`, dẫn đến một`O(n^2)`giải pháp. Với`n ≤ 50`, điều này vẫn hoàn toàn ổn trong thực tế, nhưng về mặt khái niệm, nó là chi phí không cần thiết đối với một cấu trúc đơn giản như vậy. 

Một quan sát rõ ràng hơn là chúng ta đang so sánh hai tập hợp khác nhau đúng một phần tử. Thay vì kiểm tra tư cách thành viên nhiều lần, chúng ta có thể tổng hợp sự khác biệt bằng cách sử dụng số học. Vì tất cả các giá trị đều là số nguyên nên chúng ta có thể tính tổng tất cả các nút thắt cần thiết và trừ đi tổng các nút thắt đã học. Kết quả chính xác là giá trị còn thiếu. 

Điều này có tác dụng vì mọi phần tử chung đều bị loại bỏ khi trừ hai tổng, chỉ để lại phần tử còn thiếu. 

Một quan điểm tương đương khác là sử dụng XOR. XOR tất cả các giá trị bắt buộc với tất cả các giá trị đã học cũng sẽ loại bỏ các giá trị trùng lặp, để lại giá trị bị thiếu. Cả hai cách tiếp cận đều dựa trên cùng một thuộc tính hủy, nhưng phép tính tổng đơn giản hơn và dễ đọc trực tiếp hơn. 

Phương pháp dựa trên tổng sẽ giảm vấn đề xuống còn một lần duyệt qua cả hai danh sách. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (quét tư cách thành viên) | O(n²) | O(1) | Đã chấp nhận | 
| Hủy tổng hoặc XOR | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`, sau đó đọc danh sách các nút thắt cần thiết và danh sách các nút thắt đã học. Chúng tôi giữ cả hai dưới dạng mảng số nguyên đơn giản. 
2. Tính tổng tất cả các nút thắt cần thiết. Điều này đại diện cho tổng số "khối lượng" của những gì nên có. 
3. Tính tổng các nút thắt đã học. Điều này đại diện cho những gì Sonja đã có. 
4. Trừ số tiền đã học khỏi số tiền yêu cầu. Sự khác biệt là nút bị thiếu, vì tất cả các phần tử dùng chung đều hủy chính xác một lần. 
5. Xuất ra giá trị kết quả. 

Lý do phép trừ hoạt động hiệu quả là vì mọi nút xuất hiện trong cả hai danh sách đều đóng góp như nhau vào cả hai tổng, do đó nó biến mất trong phần chênh lệch. Chỉ có một phần tử còn thiếu vẫn chưa được ghép nối. 

### Tại sao nó hoạt động 

Chúng ta đang sử dụng một cách hiệu quả thực tế rằng phép cộng trên các số nguyên có tính chất kết hợp và giao hoán, nên thứ tự không quan trọng. Nếu chúng ta viết tập hợp yêu cầu là`A`và đã học được thiết lập như`B`, và chúng tôi biết`B`chính xác là`A`không có một phần tử`x`, sau đó:`sum(A) = sum(B) + x`Sắp xếp lại mang lại`x = sum(A) - sum(B)`. Không có cấu trúc đầu vào nào khác ảnh hưởng đến nhận dạng này, vì vậy giá trị được tính toán phải chính xác là nút bị thiếu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n = int(input().strip())
    required = list(map(int, input().split()))
    learned = list(map(int, input().split()))

    total_required = sum(required)
    total_learned = sum(learned)

    print(total_required - total_learned)

if __name__ == "__main__":
    main()
```Giải pháp được chia thành ba phần logic: phân tích cú pháp đầu vào, tổng hợp và đầu ra. Chi tiết triển khai chính là đảm bảo cả hai danh sách đều được sử dụng đầy đủ dưới dạng số nguyên. Vì các ràng buộc nhỏ nên không cần phải phát trực tuyến hoặc cập nhật gia tăng. 

Một lỗi phổ biến là quên chuyển đổi chuỗi đầu vào thành số nguyên trước khi tính tổng, điều này sẽ dẫn đến nối chuỗi thay vì cộng số học trong một số ngôn ngữ. Trong Python,`map(int, ...)`tránh hoàn toàn vấn đề đó. 

Một vấn đề tế nhị khác là giả sử cả hai danh sách đều được sắp xếp hoặc căn chỉnh nhưng thực tế không phải vậy. Thuật toán cố ý tránh mọi sự phụ thuộc vào thứ tự. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
1 2 4 3
4 2 3
```Chúng tôi tính tổng từng bước. 

| Bước | Danh sách bắt buộc | Danh sách đã học | Số tiền bắt buộc | Tổng đã học | Sự khác biệt | 
| --- | --- | --- | --- | --- | --- | 
| Bắt đầu | [1,2,4,3] | [] | 0 | 0 | 0 | 
| Sau khi được yêu cầu | [1,2,4,3] | [] | 10 | 0 | 10 | 
| Sau khi học | [1,2,4,3] | [4,2,3] | 10 | 9 | 1 | 

Đầu ra:```
1
```Dấu vết này cho thấy cách tất cả các phần tử chung bị hủy, chỉ để lại giá trị còn thiếu. 

### Ví dụ 2 

đầu vào:```
3
10 101 999
1 999 101
```| Bước | Danh sách bắt buộc | Danh sách đã học | Số tiền bắt buộc | Tổng đã học | Sự khác biệt | 
| --- | --- | --- | --- | --- | --- | 
| Bắt đầu | [10,101,999] | [] | 0 | 0 | 0 | 
| Sau khi được yêu cầu | [10,101,999] | [] | 1110 | 0 | 1110 | 
| Sau khi học | [10,101,999] | [1,999,101] | 1110 | 1101 | 9 | 

Đầu ra:```
9
```Điều này chứng tỏ rằng phương pháp này không phụ thuộc vào thứ tự hoặc vị trí mà chỉ phụ thuộc vào việc hủy bỏ giá trị. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi danh sách được tóm tắt một lần trong một lần | 
| Không gian | O(1) | Chỉ một số bộ tích lũy số nguyên được sử dụng | 

Các ràng buộc cho phép tối đa 50 phần tử, do đó, ngay cả giải pháp dựa trên vòng lặp đơn giản nhất cũng có hiệu quả ngay lập tức. Thuật toán hoạt động tốt trong cả giới hạn thời gian và bộ nhớ, với chi phí hoạt động không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import main
    return sys.stdout.getvalue()

# helper for clean execution in standalone runs is omitted here

# sample 1
assert run("""4
1 2 4 3
4 2 3
""").strip() == "1"

# sample 2
assert run("""3
10 101 999
1 999 101
""").strip() == "9"

# minimum case
assert run("""2
5 7
7
""").strip() == "5"

# already near full range
assert run("""3
1 2 3
2 3
""").strip() == "1"

# unordered heavy mix
assert run("""5
100 200 300 400 500
500 300 100 200
""").strip() == "400"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 5 7 / 7 | 5 | cấu trúc hợp lệ tối thiểu | 
| 1 2 3 / 2 3 | 1 | hủy bỏ đơn giản | 
| 100 200 300 400 500 / 500 300 100 200 | 400 | tính đúng đắn không có thứ tự | 

## Vỏ cạnh 

Trường hợp một cạnh là hợp lệ nhỏ nhất`n = 2`, trong đó thiếu chính xác một giá trị. Ví dụ: 

đầu vào:```
2
5 7
7
```Tổng bắt buộc là 12, tổng đã học là 7, do đó đầu ra là 5. Thuật toán vẫn hoạt động chính xác vì phép trừ không phụ thuộc vào kích thước danh sách. 

Một trường hợp khác là khi các giá trị nằm gần ranh giới của phạm vi cho phép, chẳng hạn như 1 và 1000. Ví dụ: 

đầu vào:```
3
1 1000 500
1000 500
```Tổng bắt buộc là 1501, tổng đã học là 1500, tạo ra 1. Ngay cả các giá trị cực trị cũng không ảnh hưởng đến độ chính xác vì không liên quan đến giả định tràn hoặc thứ tự. 

Trường hợp cạnh cấu trúc cuối cùng là khi giá trị còn thiếu là giá trị lớn nhất hoặc nhỏ nhất trong tập hợp. Vì thuật toán không dựa vào việc sắp xếp hoặc lập chỉ mục nên nó xử lý tất cả các vị trí một cách thống nhất, do đó cực trị bị thiếu được xử lý giống hệt với bất kỳ phần tử nào khác.
