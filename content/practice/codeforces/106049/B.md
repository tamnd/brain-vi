---
title: "CF 106049B - Kaosar và phân đoạn"
description: "Chúng ta có một đa giác đều có các đỉnh được đánh số từ 1 đến n theo chiều kim đồng hồ. Một đoạn có thể được vẽ giữa các đỉnh i và j chỉ khi các đỉnh không kề nhau trên ranh giới đa giác và gcd(i, j) = 1."
date: "2026-06-25T12:34:53+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106049
codeforces_index: "B"
codeforces_contest_name: "TheForces Round #44 (DIV3.5-Forces)"
rating: 0
weight: 106049
solve_time_s: 42
verified: true
draft: false
---

[CF 106049B - Kaosar và Phân đoạn](https://codeforces.com/problemset/problem/106049/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 42s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đa giác đều có các đỉnh được đánh số từ`1`ĐẾN`n`theo thứ tự chiều kim đồng hồ. Một đoạn có thể được vẽ giữa các đỉnh`i`Và`j`chỉ khi các đỉnh không liền kề trên ranh giới đa giác và`gcd(i, j) = 1`. Mục tiêu là vẽ càng nhiều đoạn như vậy càng tốt trong khi vẫn giữ cho hình vẽ không giao nhau. Các đoạn được phép gặp nhau ở điểm cuối, nhưng hai đoạn không được giao nhau ở phần bên trong của chúng. 

Đầu vào chứa nhiều trường hợp thử nghiệm, mỗi trường hợp bao gồm một giá trị duy nhất`n`. Đối với mỗi trường hợp thử nghiệm, chúng tôi phải xuất ra số lượng phân đoạn tối đa có thể được thêm vào. 

Giá trị lớn nhất của`n`là`10^5`, và có thể có tới`10^4`trường hợp thử nghiệm. Bất kỳ giải pháp nào thực hiện xây dựng đồ thị hoặc hình học trên mỗi đỉnh đều không cần thiết. Chúng tôi cần một cái gì đó gần với thời gian không đổi cho mỗi trường hợp thử nghiệm. 

Một điểm tinh tế là điều kiện nguyên tố cùng nhau có vẻ quan trọng, điều này có thể cám dỗ chúng ta tìm kiếm một cấu trúc đồ thị phức tạp. Quan sát quan trọng là đỉnh đó`1`là nguyên tố cùng nhau với mọi đỉnh khác. 

Coi như`n = 3`.```
3
```Tam giác không có đường chéo nên đáp án là`0`. 

Một giải pháp bất cẩn chỉ kiểm tra điều kiện nguyên tố có thể đếm không chính xác các phân đoạn liên quan đến đỉnh`1`, quên rằng các đỉnh liền kề không thể được kết nối. 

Coi như`n = 4`. 

Đường chéo duy nhất là`(1,3)`, và nó hợp lệ vì`gcd(1,3)=1`. Câu trả lời là`1`. 

Một giải pháp cố gắng đếm tất cả các cặp nguyên tố cùng nhau sẽ đếm quá mức vì nhiều cặp hợp lệ không thể cùng tồn tại nếu không có sự giao nhau. 

## Phương pháp tiếp cận 

Cách mạnh mẽ nhất là tạo ra mọi phân đoạn hợp lệ, sau đó tìm kiếm tập hợp con không giao nhau lớn nhất. Ngay cả việc xây dựng tất cả các phân khúc ứng cử viên cũng mất khoảng`O(n²)`cặp và việc tìm tập hợp con tương thích tối đa khó hơn nhiều. Với`n = 10^5`, điều này hoàn toàn không thể thực hiện được. 

Cấu trúc của đa giác mang lại một thực tế mạnh mẽ hơn nhiều. Trong bất kỳ`n`-gon, số đường chéo không cắt nhau tối đa có thể là`n - 3`. Điều này xảy ra trong mọi tam giác. 

Vì vậy, câu hỏi trở thành: liệu chúng ta thực sự có thể đạt được`n - 3`chỉ sử dụng các phân đoạn được phép? 

Đúng. 

Lấy mọi đường chéo từ đỉnh`1`đến đỉnh`3, 4, ..., n-1`. 

Những phân đoạn này tạo thành một tam giác hình quạt. Chúng không bao giờ giao nhau vì chúng đều có chung điểm cuối. Mọi phân đoạn như vậy đều hợp lệ:`|1 - j| > 1`vì`j = 3 ... n-1`, do đó các đỉnh không liền kề nhau.`gcd(1, j) = 1`với mọi số nguyên`j`. 

Số đường chéo này chính xác là:$$(n-1) - 3 + 1 = n-3$$Vì không có cấu hình không giao nhau nào có thể chứa nhiều hơn`n-3`đường chéo và chúng tôi đã xây dựng chính xác`n-3`, điều này là tối ưu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) hoặc tệ hơn | O(n²) | Quá chậm | 
| Tối ưu | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`. 
2. Hãy nhớ lại rằng bất kỳ tập hợp đường chéo nào không cắt nhau trong một`n`-gon chứa nhiều nhất`n - 3`đường chéo. 
3. Quan sát rằng mọi đường chéo từ đỉnh`1`đến một đỉnh`j`với`3 ≤ j ≤ n-1`được cho phép bởi vì`gcd(1, j) = 1`. 
4. Các đường chéo này tạo thành hình quạt nên không có đường chéo nào cắt nhau. 
5. Số đường chéo như vậy chính xác là`n - 3`. 
6. Đầu ra`n - 3`. 

### Tại sao nó hoạt động 

Giới hạn trên xuất phát từ lý thuyết tam giác đa giác: không có tập đường chéo không cắt nào có thể chứa nhiều hơn`n - 3`đường chéo. Công trình của chúng tôi sử dụng chính xác`n - 3`đường chéo, tất cả đều thỏa mãn quy tắc tính đúng của bài toán. Vì chúng ta đồng thời đạt được giới hạn trên toàn cục và thỏa mãn mọi ràng buộc nên câu trả lời phải là`n - 3`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())

for _ in range(t):
    n = int(input())
    print(n - 3)
```Việc thực hiện cực kỳ nhỏ vì tất cả công việc đều được thực hiện bằng quan sát toán học. 

Đối với mỗi trường hợp thử nghiệm, chúng tôi xuất ra`n - 3`. Không cần hình học, xây dựng đồ thị hoặc tính toán gcd. Bằng chứng đã đảm bảo rằng quạt có tâm ở đỉnh`1`luôn tồn tại và tối ưu. 

Điều kiện biên duy nhất là`n = 3`. Trong trường hợp đó công thức cho`0`, điều này đúng vì tam giác không có đường chéo. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
```| n | Trả lời | 
| --- | --- | 
| 3 | 0 | 

Trong một tam giác không có đường chéo nên số đoạn thẳng tối đa là`0`. 

### Ví dụ 2 

đầu vào:```
5
```Cấu trúc quạt sử dụng các đường chéo`(1,3)`Và`(1,4)`. 

| Bước | Đã thêm đường chéo | Đếm | 
| --- | --- | --- | 
| 1 | (1,3) | 1 | 
| 2 | (1,4) | 2 | 

Số đếm cuối cùng là`2`, bằng`5 - 3`. 

Ví dụ này cho thấy rằng việc xây dựng đạt đến số lượng đường chéo không cắt nhau tối đa theo lý thuyết. 

### Ví dụ 3 

đầu vào:```
7
```Công dụng chế tạo quạt`(1,3)`,`(1,4)`,`(1,5)`,`(1,6)`. 

| Bước | Đã thêm đường chéo | Đếm | 
| --- | --- | --- | 
| 1 | (1,3) | 1 | 
| 2 | (1,4) | 2 | 
| 3 | (1,5) | 3 | 
| 4 | (1,6) | 4 | 

Câu trả lời là`4 = 7 - 3`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) cho mỗi trường hợp thử nghiệm | Chỉ thực hiện một phép trừ | 
| Không gian | O(1) | Không có bộ nhớ phụ | 

Ngay cả với`10^4`trường hợp thử nghiệm, thời gian chạy là không đáng kể. Giải pháp dễ dàng phù hợp trong mọi giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    t = int(input())
    ans = []

    for _ in range(t):
        n = int(input())
        ans.append(str(n - 3))

    sys.stdout.write("\n".join(ans))

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out

# provided samples
assert run("2\n3\n5\n") == "0\n2", "sample 1"

# custom cases
assert run("1\n3\n") == "0", "minimum polygon"
assert run("1\n4\n") == "1", "single diagonal"
assert run("1\n7\n") == "4", "general case"
assert run("1\n100000\n") == "99997", "maximum n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3`|`0`| Đa giác hợp lệ nhỏ nhất | 
|`4`|`1`| Tồn tại chính xác một đường chéo | 
|`7`|`4`| Cấu tạo quạt tổng hợp | 
|`100000`|`99997`| Hạn chế tối đa | 

## Vỏ cạnh 

Đối với đa giác nhỏ nhất:```
1
3
```Thuật toán tính toán`3 - 3 = 0`. 

Một hình tam giác không có đường chéo nên không có gì để vẽ. Đầu ra là chính xác. 

Đối với tứ giác:```
1
4
```Thuật toán tính toán`4 - 3 = 1`. 

Số đường chéo không vượt qua duy nhất có thể là một. Cấu tạo của quạt mang lại`(1,3)`, thỏa mãn điều kiện nguyên tố cùng nhau. 

Đối với một đa giác lớn hơn như:```
1
6
```Đầu ra của thuật toán`3`. 

Quạt bao gồm`(1,3)`,`(1,4)`, Và`(1,5)`. Tất cả đều hợp lệ vì mọi số đều nguyên tố cùng nhau`1`và chúng không giao nhau vì chúng có chung một điểm cuối. Không có giải pháp nào có thể vượt quá`6 - 3 = 3`, vậy đáp án là tối ưu.
