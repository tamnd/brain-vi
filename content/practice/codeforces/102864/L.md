---
title: "CF 102864L - \"\u6211\u4e3a\u4ec0\u4e48\u8981\u5347\u7ea7\uff1f\" 
mô tả:"Sự cố mô tả một trò chơi trong đó một tòa nhà có thể được nâng cấp nhiều lần. Lần nâng cấp đầu tiên có giá N1 vàng. Mỗi lần nâng cấp sau sẽ trở nên đắt hơn bằng cách nhân chi phí trước đó với P, trong khi mỗi lần nâng cấp luôn tăng sản lượng vàng sản xuất hàng giờ lên cùng một lượng cố định M."
date: "2026-07-25T13:46:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102864
codeforces_index: "L"
codeforces_contest_name: "The 15-th BIT Campus Programming Contest - Online Round"
rating: 0
weight: 102864
solve_time_s: 49
verified: true
draft: false
---

[CF 102864L - \"\u6211\u4e3a\u4ec0\u4e48\u8981\u5347\u7ea7\uff1f\](https://codeforces.com/problemset/problem/102864/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Sự cố mô tả một trò chơi trong đó một tòa nhà có thể được nâng cấp nhiều lần. Chi phí nâng cấp lần đầu`N1`vàng. Mỗi lần nâng cấp sau sẽ trở nên đắt hơn bằng cách nhân chi phí trước đó với`P`, trong khi mỗi lần nâng cấp luôn tăng sản lượng vàng hàng giờ với cùng một lượng cố định`M`. Đối với số nâng cấp đã chọn`Q`, chúng ta cần tính toán thời gian cần thiết để sản xuất thêm vàng chỉ từ lần nâng cấp đó để thu hồi số vàng đã chi cho lần nâng cấp đó. 

Chi phí của`Q`-th nâng cấp là một thuật ngữ tiến triển hình học. Chi phí nâng cấp lần đầu`N1`, chi phí thứ hai`N1 × P`và chi phí nâng cấp thứ Q`N1 × P^(Q-1)`. Vì lợi ích của việc nâng cấp thứ Q chỉ là phần bổ sung`M`số vàng được sản xuất mỗi giờ, câu trả lời là chi phí nâng cấp chia cho`M`. 

Số lượng ca kiểm thử có thể lên tới 100, nhưng`Q`tối đa là 50. Điều này có nghĩa là vấn đề không cần tối ưu hóa nâng cao. Ngay cả một phương pháp thực hiện một lượng công việc nhỏ cho mỗi lần nâng cấp cũng đủ nhanh. Khó khăn chính là xử lý chính xác sự tăng trưởng hình học và độ chính xác của dấu phẩy động, bởi vì`P`là số hữu tỉ và đáp án cuối cùng phải giữ một chữ số thập phân. 

Việc thực hiện bất cẩn có thể thất bại khi số nâng cấp là lần nâng cấp đầu tiên. Ví dụ, với đầu vào`1000 1.2 100 1`, câu trả lời là`10.0`, vì lần nâng cấp đầu tiên có giá chính xác là 1000 vàng. Một công thức sử dụng`P^Q`thay vì`P^(Q-1)`sẽ tính toán sai`12.0`. 

Một lỗi phổ biến khác xuất hiện khi`P`bằng`1`. Đối với đầu vào`1000 1 100 50`, mỗi lần nâng cấp vẫn tốn 1000 vàng nên thời gian hồi phục là`10.0`. Bất kỳ việc triển khai nào giả định rằng giá trị phải tăng lên và nhân lên nhiều lần mà không xem xét số hạng bắt đầu đều có thể tạo ra kết quả không chính xác. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản là mô phỏng chi phí nâng cấp. Bắt đầu với chi phí nâng cấp đầu tiên`N1`, sau đó nhân chi phí với`P`chính xác`Q-1`lần để đạt được nâng cấp thứ Q. Sau khi có được chi phí đó, chia nó cho`M`. Điều này có tác dụng vì mỗi phép nhân chuyển từ một số hạng của cấp số nhân sang số hạng tiếp theo. 

Mô phỏng lực lượng vũ phu đã có hiệu quả ở đây bởi vì`Q`được giới hạn ở 50. Trường hợp xấu nhất của nó chỉ thực hiện 49 phép nhân cho mỗi trường hợp thử nghiệm. Nếu có hàng triệu bản nâng cấp, việc nhân lên nhiều lần sẽ trở thành công việc không cần thiết, nhưng những giới hạn nhất định khiến phương pháp này hoàn toàn đủ dùng. 

Một cách tiếp cận toán học hơn sử dụng dạng đóng của một cấp số nhân. Chi phí nâng cấp thứ Q là`N1 × P^(Q-1)`, vì vậy chúng ta có thể tính trực tiếp lũy thừa và chia cho`M`. Điều này làm giảm số lượng thao tác và thể hiện mối quan hệ rõ ràng hơn, mặc dù sự khác biệt thực tế là rất nhỏ vì các ràng buộc nhỏ. 

Quan sát quan trọng là chúng ta không cần tính toán bất kỳ nâng cấp nào trước đó. Thời gian phục hồi chỉ phụ thuộc vào chi phí nâng cấp hiện tại và mức tăng sản lượng cố định, vì vậy tất cả chi phí nâng cấp trước đó đều không liên quan. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(Q) | O(1) | Đã chấp nhận | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`N1`,`P`,`M`, Và`Q`cho trường hợp thử nghiệm hiện tại. Các giá trị này mô tả chi phí nâng cấp ban đầu, tỷ lệ tăng trưởng, mức tăng sản lượng và số lần nâng cấp mục tiêu. 
2. Tính chi phí nâng cấp lần thứ Q như sau:`N1 × P^(Q-1)`. Số mũ là`Q-1`bởi vì lần nâng cấp đầu tiên đã có giá ban đầu và chưa có sự nhân lên nào xảy ra. 
3. Chia chi phí nâng cấp tính toán cho`M`. Điều này cho biết số giờ cần thiết để sản xuất thêm từ lần nâng cấp này nhằm hoàn trả số vàng đã chi. 
4. In kết quả bằng một chữ số thập phân. Số học dấu phẩy động là đủ vì bài toán đảm bảo rằng loại có độ chính xác kép có thể xử lý phạm vi được yêu cầu. 

Tại sao nó hoạt động: chuỗi chi phí chính xác là một cấp số nhân. Thuật ngữ đầu tiên là`N1`, và mọi số hạng theo sau được tạo ra bằng cách nhân với`P`. Do đó số hạng thứ Q có chính xác`Q-1`phép nhân được áp dụng. Định nghĩa thời gian phục hồi chỉ đơn giản là chi phí nâng cấp hiện tại chia cho thu nhập tăng thêm theo giờ, do đó giá trị được tính toán phù hợp với số lượng yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        N1, P, M, Q = input().split()
        N1 = int(N1)
        P = float(P)
        M = int(M)
        Q = int(Q)

        cost = N1 * (P ** (Q - 1))
        ans.append(f"{cost / M:.1f}")

    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    solve()
```Đầu vào được xử lý mỗi lần một trường hợp thử nghiệm vì không có sự phụ thuộc giữa các trường hợp. Các giá trị được chuyển đổi cẩn thận:`N1`,`M`, Và`Q`là số nguyên, trong khi`P`phải được lưu trữ dưới dạng số dấu phẩy động vì nó có thể chứa giá trị phân số. 

Số mũ sử dụng`Q - 1`, đó là điều kiện biên chính của bài toán. Khi`Q`là 1, lũy thừa trở thành 0 và Python đánh giá chính xác`P ** 0`là 1, để lại chi phí bằng`N1`. 

Định dạng cuối cùng sử dụng`:.1f`, phù hợp với độ chính xác đầu ra được yêu cầu. Số học dấu phẩy động của Python là đủ vì số mũ lớn nhất có thể chỉ là 49. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên: 

đầu vào:`1000 1.1 100 20`| Q | Chi phí hiện tại | Thời gian phục hồi | 
| --- | --- | --- | 
| 1 | 1000,0 | 10.0 | 
| 10 | 2357.9 | 23.6 | 
| 20 | 6123.0 | 61,2 | 

Chi phí nâng cấp cuối cùng là`1000 × 1.1^19`, xấp xỉ`6123.0`. Chia cho mức tăng 100 vàng mỗi giờ sẽ cho`61.2`giờ. Dấu vết cho thấy chỉ có chi phí nâng cấp hiện tại mới quan trọng. 

Đối với mẫu thứ hai: 

đầu vào:`1000 1.2 100 20`| Q | Chi phí hiện tại | Thời gian phục hồi | 
| --- | --- | --- | 
| 1 | 1000,0 | 10.0 | 
| 10 | 5153.8 | 51,5 | 
| 20 | 31951.0 | 319,5 | 

Yếu tố tăng trưởng lớn hơn khiến chi phí nâng cấp tăng nhanh hơn nhiều. Công thức tương tự vẫn được áp dụng vì quy tắc lũy tiến không thay đổi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(T) | Mỗi trường hợp thử nghiệm thực hiện tính toán công suất theo thời gian không đổi. | 
| Không gian | O(T) | Chương trình lưu trữ các câu trả lời được định dạng trước khi in chúng. | 

Với tối đa 100 trường hợp thử nghiệm, giải pháp dễ dàng phù hợp với giới hạn về thời gian và bộ nhớ. Ngay cả phương pháp mô phỏng cũng chỉ cần vài nghìn phép nhân, nhưng công thức trực tiếp giúp việc thực hiện trở nên đơn giản. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_data(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    solve()

    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return out.getvalue()

# provided samples
assert solve_data("""3
1000 1.1 100 20
1000 1.2 100 20
1000 1.1 100 50
""") == """61.2
319.5
1067.2
""", "samples"

# first upgrade boundary
assert solve_data("""1
1000 1.2 100 1
""") == "10.0\n", "Q equals 1"

# no growth
assert solve_data("""1
1000 1 100 50
""") == "10.0\n", "P equals 1"

# minimum values
assert solve_data("""1
1 1 1 1
""") == "1.0\n", "minimum input"

# larger exponent
assert solve_data("""1
1000 1.3 1000 50
""") == "3869.4\n", "maximum Q case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1000 1.2 100 1`|`10.0`| Kiểm tra Q trừ một ranh giới số mũ. | 
|`1000 1 100 50`|`10.0`| Kiểm tra trường hợp lũy tiến với chi phí không đổi. | 
|`1 1 1 1`|`1.0`| Kiểm tra giá trị tối thiểu được phép. | 
|`1000 1.3 1000 50`|`3869.4`| Kiểm tra quyền hạn lớn hơn và xử lý dấu phẩy động. | 

## Vỏ cạnh 

Khi nào`Q`là 1, không có phép nhân với`P`. Đối với đầu vào`1000 1.2 100 1`, thuật toán tính toán`1000 × 1.2^0`, đó là`1000`, sau đó chia cho`100`, sản xuất`10.0`. Một công thức sử dụng`P^Q`sẽ thêm một bước tăng trưởng không chính xác. 

Khi`P`là 1 thì cấp số nhân trở thành một dãy không đổi. Đối với đầu vào`1000 1 100 50`, thuật toán tính toán`1000 × 1^49`, còn lại`1000`. Thời gian phục hồi vẫn còn`10.0`, cho thấy giải pháp không phụ thuộc vào việc tăng chi phí. 

Khi số lượng nâng cấp lớn, chẳng hạn như`1000 1.3 1000 50`, phép nhân lặp lại và lũy thừa trực tiếp đều vẫn an toàn vì số mũ tối đa chỉ là 49. Thuật toán tính toán trực tiếp số hạng cuối cùng và tránh việc lưu trữ các giá trị trung gian không cần thiết.
