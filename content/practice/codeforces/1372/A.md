---
title: "CF 1372A - Omkar và hoàn thiện"
description: "Chúng ta được yêu cầu xây dựng một mảng số nguyên có độ dài n trong đó mọi phần tử đều dương, không vượt quá 1000 và thỏa mãn giới hạn toàn cầu về tổng: nếu chúng ta chọn bất kỳ ba vị trí nào (chúng có thể trùng nhau), thì tổng của hai phần tử được chọn không bao giờ bằng bất kỳ phần tử nào của mảng."
date: "2026-06-16T12:43:35+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1372
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 655 (Div. 2)"
rating: 800
weight: 1372
solve_time_s: 404
verified: false
draft: false
---

[CF 1372A - Omkar và Hoàn thành](https://codeforces.com/problemset/problem/1372/A) 

**Đánh giá:** 800 
**Tas:** thuật toán xây dựng, triển khai 
**Thời gian giải:** 6 phút 44s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xây dựng một mảng số nguyên có độ dài`n`trong đó mọi phần tử đều dương, không vượt quá 1000 và thỏa mãn giới hạn toàn cầu về tổng: nếu chúng ta chọn bất kỳ ba vị trí nào (chúng có thể trùng nhau), tổng của hai phần tử được chọn không bao giờ bằng bất kỳ phần tử nào của mảng. 

Nói một cách cụ thể hơn, không có giá trị nào trong mảng được phép biểu diễn dưới dạng tổng của hai giá trị từ mảng, bao gồm cả khả năng nhân đôi một phần tử. Vậy nếu`a[i] + a[j]`xuất hiện ở bất kỳ đâu trong mảng, thậm chí ở một số chỉ mục khác`k`, mảng không hợp lệ. 

Đầu vào đưa ra nhiều trường hợp kiểm thử, mỗi trường hợp chỉ cung cấp`n`và với mỗi mảng, chúng ta phải xuất ra bất kỳ mảng hợp lệ nào có độ dài đó. 

Những hạn chế là nhỏ. Tổng của tất cả`n`trên các trường hợp thử nghiệm tối đa là 1000 và mỗi trường hợp`n`nhiều nhất là 1000. Điều này ngay lập tức cho chúng ta biết rằng chúng ta không phải đối mặt với áp lực về hiệu suất. Bất kỳ giải pháp nào lên đến khoảng`O(n^2)`hoặc thậm chí tệ hơn một chút cho mỗi trường hợp thử nghiệm sẽ vượt qua một cách thoải mái, nhưng cấu trúc của điều kiện cho thấy chúng ta nên tránh hoàn toàn việc kiểm tra từng cặp không cần thiết. 

Trường hợp thất bại không rõ ràng đối với việc xây dựng đơn giản là dãy tăng dần. Nếu chúng ta cố gắng`[1, 2, 3, 4, 5]`, chúng ta nhận được ngay`1 + 2 = 3`, vi phạm điều kiện. Một nỗ lực thất bại tinh vi hơn là bất kỳ tập hợp dày đặc nào gồm các số nhỏ, vì các giá trị nhỏ tự nhiên kết hợp với các giá trị nhỏ khác cũng hiện diện. 

Cái bẫy tinh vi thứ hai là cố gắng ngẫu nhiên hóa các giá trị mà không có cấu trúc. Ngay cả khi các giá trị nằm trong giới hạn, các lựa chọn ngẫu nhiên sẽ thường xuyên tạo ra các xung đột tổng ngẫu nhiên như`a[i] + a[j] = a[k]`, đặc biệt khi số lượng nhỏ hoặc lặp lại. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng xây dựng mảng tăng dần. Đối với mỗi phần tử mới, chúng ta có thể kiểm tra tất cả các giá trị ứng viên từ`1`ĐẾN`1000`và kiểm tra xem việc thêm nó có vi phạm điều kiện với bất kỳ cặp nào đã chọn trước đó hay không. Điều này yêu cầu kiểm tra tất cả các cặp trong tiền tố hiện tại, vì vậy đối với mỗi vị trí, chúng tôi có thể thực hiện tối đa`O(n^2)`kiểm tra, dẫn đến`O(n^3)`tổng công việc cho mỗi trường hợp thử nghiệm trong trường hợp xấu nhất. Điều này là không cần thiết vì điều kiện là toàn cục và không yêu cầu duy trì cấu trúc tổ hợp chính xác một cách linh hoạt. 

Quan sát quan trọng là chúng ta không bắt buộc phải sử dụng các giá trị riêng biệt hoặc tối đa hóa sự đa dạng. Chúng ta chỉ cần tránh va chạm tổng. Một cách đơn giản để đảm bảo điều này là làm cho tất cả các phần tử bằng một giá trị không đổi. Nếu mọi phần tử đều có cùng số`x`, thì mọi tổng của hai phần tử là`2x`, không có trong mảng miễn là`x != 2x`. Từ`x > 0`, điều này luôn đúng. 

Chúng ta cũng cần tôn trọng giới hạn trên của 1000, nhưng việc chọn`x = 1`đã thỏa mãn mọi ràng buộc. Khi đó mảng bao gồm toàn bộ số 1 và tổng duy nhất có thể là`2`, không có trong mảng. 

Điều này làm cho vấn đề về cơ bản là một nhiệm vụ xây dựng liên tục. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Kiểm tra gia tăng Brute Force | O(n^3) | O(n) | Quá chậm | 
| Xây dựng mảng không đổi | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng mảng độc lập cho từng trường hợp thử nghiệm. 

1. Đọc`n`cho trường hợp thử nghiệm. Chúng ta chỉ cần xác định có bao nhiêu phần tử xuất ra chứ không phải cấu trúc của chúng ngoài việc thỏa mãn điều kiện. 
2. Chọn giá trị cố định`1`cho mọi vị trí. Điều này đảm bảo mọi phần tử đều dương và nằm trong giới hạn bắt buộc là 1000. 
3. Điền vào mảng bằng`n`bản sao của`1`. Không cần kiểm tra bổ sung vì cấu trúc đảm bảo tính hợp lệ. 
4. Xuất mảng. 

### Tại sao nó hoạt động 

Các tổng duy nhất có thể được hình thành từ mảng có dạng`1 + 1 = 2`. Vì mỗi phần tử mảng đều`1`, Và`2`không bao giờ xuất hiện trong mảng, không có bộ ba chỉ số nào sao cho`a[x] + a[y] = a[z]`. Bởi vì tất cả các phần tử đều giống hệt nhau nên mọi tổng cặp có thể đều giống nhau và được đảm bảo nằm ngoài tập hợp các giá trị được sử dụng trong mảng. Bất biến này đúng bất kể`n`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        print(" ".join(["1"] * n))

if __name__ == "__main__":
    solve()
```Việc thực hiện phản ánh trực tiếp việc xây dựng. Điều tinh tế duy nhất là xử lý nhiều trường hợp thử nghiệm và đảm bảo I/O nhanh bằng cách sử dụng`sys.stdin.readline`. 

biểu thức`["1"] * n`xây dựng mảng cần thiết một cách hiệu quả dưới dạng chuỗi, tránh chuyển đổi số nguyên thành chuỗi bên trong các vòng lặp. Vì kích thước đầu ra là yếu tố hạn chế nên điều này là tối ưu trong thực tế. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 5
```Chúng tôi xây dựng năm cái. 

| Bước | Hành động | Mảng | 
| --- | --- | --- | 
| 1 | Bắt đầu | [] | 
| 2 | Thêm 1 | [1] | 
| 3 | Thêm 1 | [1, 1] | 
| 4 | Thêm 1 | [1, 1, 1] | 
| 5 | Thêm 1 | [1, 1, 1, 1] | 
| 6 | Thêm 1 | [1, 1, 1, 1, 1] | 

Tất cả các cặp tổng bằng 2, không nằm trong mảng, xác nhận tính hợp lệ. 

### Ví dụ 2 

đầu vào:```
n = 4
```| Bước | Hành động | Mảng | 
| --- | --- | --- | 
| 1 | Bắt đầu | [] | 
| 2 | Thêm 1 | [1] | 
| 3 | Thêm 1 | [1, 1] | 
| 4 | Thêm 1 | [1, 1, 1] | 
| 5 | Thêm 1 | [1, 1, 1, 1] | 

Một lần nữa, tất cả các tổng đều bằng 2, không xuất hiện trong mảng, do đó điều kiện được giữ nguyên. 

Những dấu vết này cho thấy rằng không có sự tương tác nào giữa các phần tử tạo ra sự bình đẳng bị cấm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Chúng tôi xuất ra chính xác`n`giá trị và thực hiện công việc liên tục trên mỗi giá trị | 
| Không gian | O(1) thêm | Ngoài bộ đệm đầu ra, không có cấu trúc dữ liệu bổ sung nào được sử dụng | 

Tổng kích thước đầu ra trên tất cả các trường hợp thử nghiệm tối đa là 1000 phần tử, do đó việc xây dựng diễn ra nhanh chóng trong các điều kiện ràng buộc. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        out.append(" ".join(["1"] * n))
    return "\n".join(out)

# provided samples (format adapted since output is not unique)
assert run("2\n5\n4\n") == "1 1 1 1 1\n1 1 1 1", "sample-like check"

# custom cases
assert run("1\n1\n") == "1", "minimum size"
assert run("1\n10\n") == "1 1 1 1 1 1 1 1 1 1", "uniform larger case"
assert run("1\n1000\n") == " ".join(["1"] * 1000), "maximum n case"
assert run("3\n2\n3\n4\n") == "1 1\n1 1 1\n1 1 1 1", "multiple small cases"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`n=1`|`[1]`| xây dựng hợp lệ tối thiểu | 
|`n=1000`| 1000 cái | xử lý kích thước tối đa | 
| nhiều trường hợp thử nghiệm | những cái lặp đi lặp lại | tính đúng đắn của nhiều trường hợp | 

## Vỏ cạnh 

Trường hợp cạnh có ý nghĩa duy nhất là khi`n = 1`. Việc xây dựng vẫn đầu ra`[1]`. Không có cặp nào để tạo thành tổng nên điều kiện được thỏa mãn. 

Vì`n = 1000`, chúng tôi xuất ra 1000 đơn vị. Dù có nhiều cặp nhưng mỗi tổng vẫn còn`2`, không có trong mảng nên không phát sinh vi phạm. 

Trường hợp các ca kiểm thử lặp lại không thay đổi hành vi vì mỗi ca kiểm thử đều độc lập và sử dụng cùng một logic xây dựng.
