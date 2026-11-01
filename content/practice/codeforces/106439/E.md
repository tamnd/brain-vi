---
title: "CF 106439E - Dư âm"
description: "Nhiệm vụ là xây dựng một mảng số nguyên dương có độ dài n thỏa mãn quy tắc cục bộ giữa mọi cặp lân cận. Với mỗi vị trí i, giá trị tại i phải để số dư đúng 1 khi chia cho giá trị tiếp theo."
date: "2026-06-25T09:30:20+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106439
codeforces_index: "E"
codeforces_contest_name: "Insomnia-26"
rating: 0
weight: 106439
solve_time_s: 34
verified: true
draft: false
---

[CF 106439E - Dư âm](https://codeforces.com/problemset/problem/106439/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 34s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ là xây dựng một mảng số nguyên dương có độ dài`n`thỏa mãn một quy tắc cục bộ giữa mỗi cặp lân cận. Đối với mỗi vị trí`i`, giá trị tại`i`phải để lại chính xác phần còn lại`1`khi chia cho giá trị tiếp theo. Trong số tất cả các mảng hợp lệ, chúng ta cần mảng nhỏ nhất về mặt từ điển, nghĩa là vị trí đầu tiên nơi hai mảng khác nhau phải chứa giá trị nhỏ hơn trong mảng đã chọn. 

Đầu vào chứa một số trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm chỉ cung cấp độ dài cần thiết của mảng. Đầu ra là trình tự được xây dựng cho độ dài đó. Các ràng buộc cho phép lên đến`10^5`các ca kiểm thử và tổng độ dài của tất cả các ca kiểm thử nhiều nhất là`5 * 10^5`, do đó, một thuật toán tiêu tốn nhiều thời gian tuyến tính hơn cho mỗi phần tử sẽ là không cần thiết. Cách tiếp cận bậc hai, chẳng hạn như thử nhiều giá trị có thể cho mọi vị trí, sẽ nhanh chóng vượt quá các phép toán có sẵn. 

Các trường hợp cạnh chính xuất phát từ hành vi bất thường của modulo với ước số nhỏ. 

Vì`n = 1`, không có cặp liền kề nào, vì vậy mọi số nguyên dương đều có tác dụng. Lựa chọn nhỏ nhất là:```
Input:
1

Output:
1
```Một giải pháp bất cẩn luôn cố gắng xây dựng một chuỗi giảm dần sẽ thất bại ở đây vì không có yếu tố thứ hai hạn chế yếu tố thứ nhất. 

Đối với mảng lớn hơn, giá trị cuối cùng không thể`1`. Nếu một số phần tử sau vị trí đầu tiên là`1`, phần tử trước đó sẽ cần phải thỏa mãn`x mod 1 = 1`, điều này là không thể vì mọi số nguyên modulo`1`là`0`. Ví dụ:```
Input:
3
```Câu trả lời hợp lệ là:```
1 3 2
```bởi vì`1 mod 3`không được kiểm tra và`3 mod 2 = 1`. 

Một cách tiếp cận sai lầm cố gắng giảm thiểu mọi phần tử một cách độc lập có thể chọn một hậu tố kết thúc bằng`1`, nhưng hậu tố đó không bao giờ có thể được mở rộng thành một mảng hợp lệ. 

## Phương pháp tiếp cận 

Ý tưởng mạnh mẽ trực tiếp sẽ là tìm kiếm phần tử đầu tiên nhỏ nhất có thể, sau đó là phần tử thứ hai nhỏ nhất có thể và tiếp tục trong khi kiểm tra xem các vị trí còn lại có còn được lấp đầy hay không. Điều này đúng về mặt logic vì thứ tự từ điển được quyết định từ trái sang phải. Tuy nhiên, không có phạm vi giá trị ứng cử viên cố định nhỏ và việc khám phá các khả năng nhanh chóng trở nên tốn kém. Với chiều dài lên đến`5 * 10^5`, ngay cả việc tìm kiếm phân nhánh khiêm tốn cũng không thể thực hiện được. 

Quan sát hữu ích xuất phát từ việc xem xét chính điều kiện modulo. Đối với mọi vị trí sau vị trí đầu tiên, giá trị tiếp theo không thể được`1`, vậy mọi phần tử từ`a2`trở đi ít nhất là`2`. Nếu cả hai số trong`x mod y = 1`ít nhất là`2`, sau đó`x`phải lớn hơn`y`, vì số nhỏ hơn không thể có số dư khác 0 sau khi chia cho số lớn hơn. 

Điều này có nghĩa là hậu tố`a2, a3, ..., an`phải luôn luôn giảm nghiêm ngặt. Giá trị nhỏ nhất có thể có ở cuối là`2`, bởi vì`1`bị cấm. Một lần`an = 2`được cố định, giá trị hợp lệ nhỏ nhất trước đó là`3`, sau đó`4`, vân vân. 

Yếu tố đầu tiên là đặc biệt. Nó không nhất thiết phải lớn hơn phần tử thứ hai vì nó chỉ cần thỏa mãn quy tắc modulo với`a2`. Giá trị đầu tiên nhỏ nhất có thể là`1`, Và`1 mod anything greater than 1`luôn luôn là`1`. Do đó việc xây dựng tối ưu bắt đầu với`1`và sau đó đặt chuỗi giảm dần từ`n`xuống`2`. 

Mảng cuối cùng là:```
1, n, n-1, ..., 2
```Việc xây dựng trực tiếp đưa ra tiền tố nhỏ nhất có thể và hậu tố hợp lệ nhỏ nhất có thể. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ trong trường hợp xấu nhất | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) để lưu trữ đầu ra | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc độ dài`n`của mảng được yêu cầu. 

Việc xây dựng chỉ phụ thuộc vào kích thước, do đó không cần tìm kiếm hoặc xử lý trước bổ sung. 
2. Đặt`1`làm phần tử đầu tiên. 

Đây là số nguyên dương nhỏ nhất có thể. Vì phần tử tiếp theo sẽ ít nhất`2`, điều kiện`1 mod a2 = 1`được tự động thỏa mãn. 
3. Nối tất cả các số từ`n`xuống`2`. 

Điều này tạo ra một hậu tố giảm nghiêm ngặt. Mọi cặp liền kề trong hậu tố này đều có dạng`x`theo sau là`x - 1`. Từ`x mod (x - 1) = 1`, mọi chuyển đổi đều hợp lệ. 
4. Xuất ra chuỗi đã xây dựng. 

Tại sao nó hoạt động: 

Hậu tố bắt đầu từ phần tử thứ hai phải giảm nghiêm ngặt, do đó các giá trị nhỏ nhất có thể có của nó chính xác là`n, n-1, ..., 2`. Bất kỳ giá trị nhỏ hơn nào ở một số vị trí sẽ buộc phải trùng lặp hoặc yêu cầu sử dụng`1`, không thể xuất hiện trong hậu tố. Giá trị đầu tiên có thể được giảm độc lập thành`1`và không có mảng hợp lệ nào có thể bắt đầu bằng số nguyên dương nhỏ hơn. Do đó việc xây dựng là tối thiểu về mặt từ điển. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        n = int(input())
        cur = [1]
        for x in range(n, 1, -1):
            cur.append(x)
        ans.append(" ".join(map(str, cur)))

    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    solve()
```Mã sẽ đọc tất cả các trường hợp kiểm thử trước và xây dựng từng câu trả lời một cách độc lập. Danh sách bắt đầu bằng`1`vì vị trí đầu tiên có vai trò khác với phần còn lại của chuỗi. 

Vòng lặp nối thêm các giá trị từ`n`xuống`2`. Có chính xác`n - 1`lặp lại, do đó chuỗi được tạo có độ dài cần thiết. Thứ tự giảm dần cũng tránh được bất kỳ sai sót nào xung quanh phần tử cuối cùng vì vòng lặp dừng chính xác trước khi đến phần tử cuối cùng.`1`, không thể xuất hiện trong hậu tố. 

Số nguyên Python không có vấn đề tràn ở đây vì giá trị được tạo lớn nhất chỉ`5 * 10^5`. 

## Ví dụ đã hoạt động 

cho`n = 1`: 

| Bước | Công trình hiện tại | Giá trị gia tăng | 
| --- | --- | --- | 
| Bắt đầu |`[1]`| không | 

Câu trả lời là:```
1
```Điều này thể hiện trường hợp ràng buộc trống trong đó không cần kiểm tra mối quan hệ modulo. 

Vì`n = 4`: 

| Bước | Công trình hiện tại | Giá trị gia tăng | 
| --- | --- | --- | 
| Bắt đầu |`[1]`|`1`| 
| Nối giá trị hậu tố lớn nhất |`[1, 4]`|`4`| 
| Tiếp tục giảm |`[1, 4, 3]`|`3`| 
| Hậu tố kết thúc |`[1, 4, 3, 2]`|`2`| 

Câu trả lời cuối cùng là:```
1 4 3 2
```Việc kiểm tra modulo là:```
1 mod 4 = 1
4 mod 3 = 1
3 mod 2 = 1
```Dấu vết này xác nhận tính bất biến rằng mọi chuyển đổi sau phần tử đầu tiên đều nằm giữa các số giảm liên tiếp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi phần tử đầu ra được tạo chính xác một lần. | 
| Không gian | O(n) | Trình tự được lưu trữ trước khi in. | 

Tổng số tiền của tất cả`n`các giá trị được giới hạn bởi`5 * 10^5`, do đó chương trình hoàn chỉnh chỉ thực hiện một số thao tác tuyến tính và vừa vặn trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

def generate(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        arr = [1]
        for x in range(n, 1, -1):
            arr.append(x)
        out.append(" ".join(map(str, arr)))

    return "\n".join(out)

assert generate("""2
1
2
""") == """1
1 2""", "samples"

assert generate("""1
1
""") == "1", "minimum size"

assert generate("""1
5
""") == "1 5 4 3 2", "larger chain"

assert generate("""1
10
""") == "1 10 9 8 7 6 5 4 3 2", "maximum style ordering"

assert generate("""1
3
""") == "1 3 2", "small boundary case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`1`| Xử lý trường hợp phần tử đơn một cách chính xác. | 
|`2`|`1 2`| Kiểm tra mối quan hệ modulo không tầm thường nhỏ nhất. | 
|`5`|`1 5 4 3 2`| Xác minh việc xây dựng hậu tố giảm dần. | 
|`10`|`1 10 9 8 7 6 5 4 3 2`| Kiểm tra việc tạo đầu ra lớn hơn. | 
|`3`|`1 3 2`| Xác nhận hậu tố nhỏ nhất vẫn hoạt động. | 

## Vỏ cạnh 

Đối với trường hợp phần tử đơn:```
Input:
1
```Các đầu ra thuật toán:```
1
```Không có cặp liền kề nên không có điều kiện modulo nào có thể sai. Lựa chọn`1`là tối thiểu về mặt từ điển. 

Đối với một chuỗi có giá trị hậu tố là`1`có vẻ hấp dẫn:```
Input:
3
```Thuật toán tạo ra:```
1 3 2
```Yếu tố thứ hai là`3`, không`1`, bởi vì bất kỳ phần tử nào sau phần tử đầu tiên đều không thể`1`. Việc kiểm tra là:```
1 mod 3 = 1
3 mod 2 = 1
```Việc xây dựng tránh được tình huống không hợp lệ trong đó một số giá trị trước đó cần phải đáp ứng modulo`1`. 

Đối với một ví dụ lớn hơn:```
Input:
6
```Thuật toán tạo ra:```
1 6 5 4 3 2
```Hậu tố giảm dần và mỗi cặp sau vị trí đầu tiên đều bao gồm các số liên tiếp. Với mọi số nguyên`k > 1`,`(k + 1) mod k = 1`, vì vậy tất cả các chuyển đổi được yêu cầu đều hợp lệ. Giá trị đầu tiên vẫn ở mức tối thiểu, giúp duy trì tính tối ưu về mặt từ điển.
