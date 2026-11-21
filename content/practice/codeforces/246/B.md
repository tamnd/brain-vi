---
title: "CF 246B - Tăng giảm"
description: "Chúng ta có một mảng các số nguyên và chúng ta được phép chọn hai phần tử khác nhau, đồng thời tăng từng phần tử 1 và giảm phần tử kia đi 1. Chúng ta có thể thực hiện thao tác này bao nhiêu lần tùy thích."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 246
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 151 (Div. 2)"
rating: 1300
weight: 246
solve_time_s: 58
verified: true
draft: false
---

[CF 246B - Tăng và Giảm](https://codeforces.com/problemset/problem/246/B) 

**Đánh giá:** 1300 
**Tags:** tham lam, toán học 
**Thời gian giải:** 58s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một mảng các số nguyên và chúng ta được phép chọn hai phần tử khác nhau, đồng thời tăng từng phần tử 1 và giảm phần tử kia đi 1. Chúng ta có thể thực hiện thao tác này bao nhiêu lần tùy thích. Nhiệm vụ là xác định số lượng phần tử mảng lớn nhất có thể bằng nhau sau bất kỳ chuỗi thao tác nào. 

Điều quan trọng là giải thích hoạt động này làm gì. Mỗi thao tác bảo toàn tổng của mảng, vì việc tăng một số lên 1 và giảm một số khác đi 1 sẽ hủy bỏ. Vì vậy, tổng là bất biến. Đồng thời, thao tác này cho phép “phân phối lại” các giá trị: số lượng lớn có thể giảm đi trong khi số lượng nhỏ hơn lại tăng lên. Điều này gợi ý rằng số lượng phần tử bằng nhau tối đa sẽ xảy ra khi mảng cân bằng nhất có thể xung quanh một số giá trị mục tiêu. 

Mảng có thể có tới 100.000 phần tử và có giá trị lên tới 10.000 độ lớn. Điều này loại trừ bất kỳ mô phỏng chuyển động mạnh mẽ nào vì việc đó có thể mất tới$10^9$các bước nếu chúng tôi cố gắng áp dụng nhiều lần các thao tác. 

Các trường hợp cạnh bao gồm các mảng có tất cả các phần tử bằng nhau, mảng chỉ có một phần tử hoặc mảng có hai phần tử khác nhau nhiều hơn 1. Ví dụ: một mảng`[2, 1]`không thể hoàn toàn bằng nhau vì tổng của chúng bằng 3 và không có số nguyên`x`như vậy`2x = 3`cho cả hai phần tử. Việc xử lý chính xác những trường hợp như vậy đòi hỏi phải xem xét tổng của các thuộc tính mảng và phép chia số nguyên chứ không chỉ so sánh các phần tử riêng lẻ. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản sẽ cố gắng mô phỏng mọi cặp phép toán có thể có, điều chỉnh các phần tử theo hướng bằng nhau cho đến khi không còn phép toán nào trợ giúp nữa. Về nguyên tắc, điều này đúng vì phép toán bảo toàn tổng và di chuyển các giá trị về phía nhau. Nhưng trong trường hợp xấu nhất, mỗi lần di chuyển chỉ thay đổi hai phần tử đi 1. Nếu mảng chứa 100.000 phần tử có giá trị thứ tự 10.000, thì điều này có thể yêu cầu hàng tỷ thao tác, quá chậm. 

Cái nhìn sâu sắc để đơn giản hóa vấn đề xuất phát từ việc quan sát hai điều. Đầu tiên, tổng của mảng không thay đổi. Thứ hai, nếu chúng ta muốn`k`các phần tử bằng một số nguyên`x`, sau đó`k * x`phải nhỏ hơn hoặc bằng tổng số tiền, nếu không sẽ không có đủ tổng để phân phối. Từ đó, chúng ta có thể thấy rằng số phần tử bằng nhau tối đa có liên quan chặt chẽ đến ước chung lớn nhất của tổng trên kích thước mảng, nhưng về mặt thực tế, chúng ta chỉ cần kiểm tra xem tổng có chia hết cho không`n`hay không. Nếu tổng đó chia hết cho`n`, chúng ta có thể làm cho tất cả các phần tử bằng`sum // n`. Nếu không, điều tốt nhất chúng ta có thể làm là`n - 1`phần tử bằng nhau, vì một phần tử sẽ phải hấp thụ phần còn lại. 

Câu chuyện ở đây là brute-force phát huy tác dụng vì mỗi thao tác đều hướng tới sự bình đẳng, nhưng lại thất bại vì có quá nhiều thao tác. Quan sát rằng tổng là bất biến và đẳng thức yêu cầu các giá trị nguyên cho phép chúng ta giảm điều này thành một phép kiểm tra đơn giản dựa trên khả năng chia hết. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n * max(a)) | O(n) | Quá chậm | 
| Dựa trên toán học tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng các phần tử trong mảng. Điều này thể hiện tổng số “tài nguyên” mà chúng ta có thể phân phối lại. 
2. Kiểm tra xem tổng có chia hết cho số phần tử không`n`. Nếu đúng như vậy thì chúng ta có thể làm cho tất cả các phần tử bằng`sum // n`. Điều này hoạt động vì mỗi thao tác bảo toàn tổng và phép chia số nguyên đảm bảo tồn tại giá trị mục tiêu chính xác. 
3. Nếu tổng không chia hết cho`n`, thì không thể làm cho tất cả các phần tử bằng nhau. Trong trường hợp này, điều tốt nhất chúng ta có thể làm là thực hiện`n - 1`các phần tử bằng nhau. Phần tử còn lại sẽ hấp thụ phần còn lại. Điều này tuân theo nguyên tắc chuồng bồ câu: tổng của`n-1`các yếu tố có thể được cân bằng chính xác, để lại một yếu tố phụ. 
4. In kết quả dựa vào phép chia. 

Tại sao nó hoạt động: bất biến là tổng của mảng. Bất kỳ chuỗi phép toán hợp lệ nào đều bảo toàn tổng này, do đó giá trị đích cho các phần tử bằng nhau phải đáp ứng`k * x <= sum`cho số nguyên`x`. Tính chia hết đảm bảo số tiền có thể được chia đều; nếu không thì một phần tử phải lấy phần còn lại. Không có cấu hình nào khác có thể tạo ra nhiều phần tử bằng nhau hơn vì tăng vượt quá`n - 1`các phần tử sẽ vi phạm ràng buộc tổng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))

total = sum(a)

if total % n == 0:
    print(n)
else:
    print(n - 1)
```Đầu tiên, mã sẽ đọc số phần tử và mảng. Nó tính tổng và sau đó kiểm tra khả năng chia hết cho`n`. Việc sử dụng phép chia số nguyên trực tiếp là không cần thiết vì chúng ta chỉ cần biết tổng có chia đều hay không. các`n - 1`dự phòng xử lý các trường hợp trong đó một phần còn lại ngăn cản sự bình đẳng hoàn toàn. Không có vấn đề về ranh giới vì Python tự động xử lý các số nguyên lớn. 

## Ví dụ đã hoạt động 

**Ví dụ 1** 

đầu vào:```
2
2 1
```| Bước | tổng cộng | tổng cộng % n | Kết quả | 
| --- | --- | --- | --- | 
| Tính tổng | 3 | 3 % 2 = 1 | tổng không chia hết | 
| Đầu ra | - | - | n - 1 = 1 | 

Giải thích: Tổng bằng 3. Hai số nguyên bằng nhau không thể có tổng bằng 3 nên chỉ một phần tử có thể khớp với phần tử khác. 

**Ví dụ 2** 

đầu vào:```
3
4 4 2
```| Bước | tổng cộng | tổng cộng % n | Kết quả | 
| --- | --- | --- | --- | 
| Tính tổng | 10 | 10% 3 = 1 | tổng không chia hết | 
| Đầu ra | - | - | n - 1 = 2 | 

Giải thích: Tổng 10 không thể chia đều cho 3 nên các phần tử bằng nhau tối đa là 2. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lần để tính tổng mảng | 
| Không gian | O(1) | Chỉ lưu tổng tổng và một vài số nguyên | 

Sự phức tạp này dễ dàng phù hợp với các ràng buộc của bài toán. Với`n`lên tới 100.000, một lần quét tuyến tính thực hiện tốt dưới 2 giây. Việc sử dụng bộ nhớ ở mức tối thiểu và thấp hơn nhiều so với 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    a = list(map(int, input().split()))
    total = sum(a)
    return str(n if total % n == 0 else n - 1)

# Provided sample
assert run("2\n2 1\n") == "1", "sample 1"

# Custom tests
assert run("1\n100\n") == "1", "single element"
assert run("3\n1 1 1\n") == "3", "all equal"
assert run("4\n2 2 2 3\n") == "3", "one remainder"
assert run("5\n5 5 5 5 5\n") == "5", "all equal large n"
assert run("2\n-100 100\n") == "2", "sum divisible with negatives"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n100`| 1 | Mảng một phần tử | 
|`3\n1 1 1`| 3 | Tất cả đều bình đẳng | 
|`4\n2 2 2 3`| 3 | Một yếu tố khác nhau | 
|`5\n5 5 5 5 5`| 5 | Tất cả đều bằng nhau, lớn hơn n | 
|`2\n-100 100`| 2 | Giá trị âm, tổng chia hết | 

## Vỏ cạnh 

Nếu mảng chỉ có một phần tử thì tổng có thể chia hết cho`n=1`và thuật toán xuất ra chính xác 1. Đối với các mảng có hai phần tử khác nhau 1, như`[2, 1]`, tổng 3 không chia hết cho 2 nên thuật toán cho kết quả là 1, điều này đúng. Đối với các mảng có tất cả các phần tử bằng nhau, tổng chia đều nên kết quả là`n`. Thuật toán xử lý các số âm và số 0 một cách tự nhiên vì logic tổng áp dụng thống nhất cho các số nguyên.
