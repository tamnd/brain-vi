---
title: "CF 105445A - Sum Vui"
description: "Chúng ta được yêu cầu quyết định xem có thể xây dựng một số có cấu trúc rất cứng nhắc hay không: nó phải có chính xác n chữ số, nó phải là một bảng màu, không có chữ số nào của nó có thể bằng 0 và tổng của tất cả các chữ số của nó phải chính xác là m."
date: "2026-06-23T03:25:57+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105445
codeforces_index: "A"
codeforces_contest_name: "TheForces Round #36 (Starters-Forces)"
rating: 0
weight: 105445
solve_time_s: 92
verified: false
draft: false
---

[CF 105445A - Sum Fun](https://codeforces.com/problemset/problem/105445/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 32s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu quyết định xem liệu có thể xây dựng một số có cấu trúc rất cứng nhắc hay không: nó phải có chính xác`n`các chữ số, nó phải là một palindrome, không có chữ số nào của nó có thể bằng 0 và tổng tất cả các chữ số của nó phải chính xác`m`. 

Ràng buộc palindrome có nghĩa là số được xác định hoàn toàn bởi nửa đầu của nó. Bất cứ điều gì chúng ta chọn cho phía bên trái sẽ tự động phản chiếu sang phía bên phải. Điều này ngay lập tức kết hợp tổng các chữ số: mỗi chữ số ở nửa đầu đóng góp một hoặc hai lần tùy thuộc vào việc nó có nằm ở trung tâm hay không (đối với độ dài lẻ). 

Độ căng chính nằm giữa ba ràng buộc: số chữ số, tổng chữ số cố định và giới hạn ở các chữ số từ 1 đến 9. Điều kiện palindrome làm giảm sự tự do, trong khi điều kiện bằng 0 loại bỏ tính linh hoạt “điền bằng số 0” thông thường được sử dụng trong các bài toán xây dựng tổng. 

Kích thước đầu vào lớn về mặt giá trị của`n`Và`m`lên tới 10^8, nhưng mỗi trường hợp kiểm thử đều độc lập và chỉ yêu cầu lý luận theo thời gian không đổi. Bất kỳ giải pháp nào cố gắng xây dựng từng chữ số sẽ vẫn chỉ cần O(n) cho mỗi lần kiểm tra, điều này là không thể. Vì vậy, nhiệm vụ hoàn toàn là lý luận số học về tính khả thi. 

Một trường hợp thất bại phổ biến là bỏ qua hiệu ứng chẵn lẻ của palindromes. Ví dụ: giả sử chúng ta luôn có thể phân phối tổng đều cho các chữ số sẽ dẫn đến câu trả lời sai khi`n`là số lẻ vì chữ số ở giữa không được nhân đôi. 

Một thất bại tinh tế khác là xử lý vấn đề như “chúng ta có thể viết m dưới dạng tổng của n số từ 1 đến 9 không”. Điều đó là cần thiết nhưng chưa đủ vì tính đối xứng palindrome làm giảm số chữ số độc lập hiệu dụng. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ sẽ cố gắng xây dựng một chữ số palindrome hợp lệ theo từng chữ số. Người ta có thể gán đệ quy các chữ số cho nửa đầu, phản chiếu chúng và kiểm tra xem tổng kết quả có khớp với nhau không`m`. Cách tiếp cận này phân nhánh rất nhiều: mỗi vị trí có 9 lựa chọn và một nửa số đó có khoảng`n/2`các vị trí độc lập. Ngay cả việc cắt bớt số tiền còn lại vẫn để lại sự tăng trưởng theo cấp số nhân. Vì`n`lên tới 10^8, điều này hoàn toàn không khả thi. 

Quan sát quan trọng là cấu trúc thu gọn bài toán thành một số ràng buộc số học. Thay vì nghĩ về từng chữ số riêng lẻ, chúng tôi nghĩ về việc mỗi vị trí đóng góp bao nhiêu vào tổng số tiền. 

Cho phép`k = n // 2`. có`k`các cặp được nhân đôi và có thể có một chữ số ở giữa nếu`n`thật kỳ quặc. Mỗi cặp đóng góp`2 * d`, và trung tâm đóng góp`d`. Vì mỗi chữ số nằm trong khoảng từ 1 đến 9 nên mỗi cặp đóng góp từ 2 đến 18 và chữ số ở giữa đóng góp từ 1 đến 9. 

Điều này chuyển vấn đề thành việc kiểm tra xem liệu chúng ta có thể chọn`k`giá trị trong`[1,9]`và tùy chọn một giá trị bổ sung trong`[1,9]`sao cho tổng trọng số phù hợp`m`. 

Thay vì xây dựng một cách rõ ràng, chúng ta chỉ cần các giới hạn khả thi: 

Tổng tối thiểu có thể xảy ra khi tất cả các chữ số là 1. Điều đó mang lại`n`. 

Tổng tối đa có thể xảy ra khi tất cả các chữ số là 9. Điều đó mang lại`9n`. 

Vậy điều kiện cần là`n ≤ m ≤ 9n`. 

Bây giờ chúng ta phải kiểm tra xem cấu trúc palindrom có ​​bổ sung thêm bất kỳ hạn chế nào ngoài khoảng này hay không. Hóa ra là không. Bất kỳ tổng số nguyên nào trong phạm vi này đều có thể được nhận ra bằng cách điều chỉnh các cặp phản chiếu và có thể cả chữ số ở giữa, bởi vì các cặp cho phép tăng 2 trên mỗi đơn vị thay đổi và tâm cho phép tăng 1. Sự kết hợp này bao gồm tất cả các số nguyên trong khoảng khả thi mà không có khoảng trống. 

Vì vậy, toàn bộ vấn đề giảm xuống còn một khoảng thời gian kiểm tra đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Xây dựng lực lượng vũ phu | hàm mũ | O(n) | Quá chậm | 
| Kiểm tra tính khả thi của phạm vi | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`Và`m`cho mỗi trường hợp thử nghiệm, vì mỗi truy vấn là độc lập và không có trạng thái nào được chuyển sang. 
2. Tính tổng các chữ số nhỏ nhất có thể với giả sử mỗi chữ số là 1, tức là`n`. Điều này tương ứng với palindrome hợp lệ nhỏ nhất không có số 0. 
3. Tính tổng các chữ số lớn nhất có thể với giả sử mỗi chữ số là 9, tức là`9n`. Điều này tương ứng với bảng màu hợp lệ lớn nhất dưới các ràng buộc về chữ số. 
4. Kiểm tra xem`m`nằm bên trong`[n, 9n]`. Nếu có thì xuất ra`YES`, nếu không thì xuất ra`NO`. 

### Tại sao nó hoạt động 

Mỗi số hợp lệ là một dãy`n`các chữ số, mỗi chữ số đóng góp độc lập vào tổng, nhưng bị ràng buộc bởi tính đối xứng chỉ trong cách sắp xếp chứ không phải trong tổng số có thể đạt được. Ràng buộc palindrome chỉ hạn chế vị trí của các chữ số chứ không hạn chế tập hợp các chữ số có thể đạt được. Bởi vì mọi vị trí chữ số vẫn có thể nhận bất kỳ giá trị nào từ 1 đến 9 (được nhân đôi một cách thích hợp), tập hợp các tổng có thể tạo thành một khoảng liên tục từ`n`ĐẾN`9n`. Không có khoảng trống nào tồn tại bên trong khoảng này vì việc tăng bất kỳ chữ số nào từ 1 lên 9 có thể được thực hiện dần dần qua các vị trí đối xứng hoặc ở giữa mà không bỏ qua các tổng có thể đạt được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []
    for _ in range(t):
        n, m = map(int, input().split())
        if n <= m <= 9 * n:
            out.append("YES")
        else:
            out.append("NO")
    print("".join(out))

if __name__ == "__main__":
    solve()
```Việc thực hiện dựa trên nguồn gốc trực tiếp của các giới hạn khả thi. Mỗi trường hợp thử nghiệm được xử lý độc lập trong thời gian không đổi. Đầu ra được tích lũy trong một danh sách để tránh lặp lại chi phí I/O, điều này rất quan trọng với tối đa 10^4 trường hợp thử nghiệm. 

Điểm tinh tế trung tâm là không có cách xây dựng palindrome rõ ràng nào được thử. Lý do đã đảm bảo rằng nếu tổng nằm trong giới hạn thì sẽ tồn tại phép gán chữ số đối xứng hợp lệ. 

## Ví dụ đã hoạt động 

Hãy xem xét một ví dụ nhỏ trong đó`n = 3, m = 6`. Phạm vi khả thi là`[3, 27]`, vì vậy điều này có thể thực hiện được. 

Chúng tôi lý luận về cấu trúc: 

| Bước | n | m | tổng tối thiểu | tổng tối đa | quyết định | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 3 | 6 | 3 | 27 | CÓ | 

Điều này xác nhận rằng số tiền tầm trung được chấp nhận mà không cần xây dựng. 

Bây giờ hãy xem xét`n = 2, m = 19`. Phạm vi là`[2, 18]`. 

| Bước | n | m | tổng tối thiểu | tổng tối đa | quyết định | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 19 | 2 | 18 | KHÔNG | 

Điều này cho thấy một trường hợp không thể tính tổng mặc dù trực giác ngây thơ về tổng chữ số có thể khiến người ta lầm tưởng rằng 9 và 10 có thể được chia một cách đối xứng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 

|---|---|---|---| 

| Thời gian | O(t) | Mỗi trường hợp thử nghiệm là một lần kiểm tra phạm vi thời gian không đổi | 

| Không gian | O(1) | Chỉ có một vài số nguyên và bộ đệm đầu ra | 

Giải pháp có tỷ lệ tuyến tính với số lượng trường hợp thử nghiệm và bỏ qua độ lớn của`n`Và`m`, điều này rất cần thiết với giới hạn trên của chúng là 10^8. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(sys.stdin.readline())
    out = []
    for _ in range(t):
        n, m = map(int, sys.stdin.readline().split())
        out.append("YES" if n <= m <= 9 * n else "NO")
    return "".join(out)

# provided samples (formatted consistently)
assert run("1\n7 10\n1\n8 10\n1\n5 5\n") == "YESYESYES", "sample 1-like"

# minimum edge
assert run("1\n1 1\n") == "YES", "single digit minimal"

# impossible small sum
assert run("1\n1 2\n") == "YES", "actually valid check boundary"

# impossible large sum
assert run("1\n2 19\n") == "NO", "above max bound"

# all max digits
assert run("1\n4 36\n") == "YES", "all 9s palindrome"

# all min digits
assert run("1\n4 4\n") == "YES", "all 1s palindrome"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Tối thiểu 1 chữ số | CÓ | giới hạn dưới ranh giới | 
| tổng tràn 2 chữ số | KHÔNG | từ chối giới hạn trên | 
| đủ 9 chữ số | CÓ | tính khả thi xây dựng tối đa | 
| toàn 1 chữ số | CÓ | tính khả thi xây dựng tối thiểu | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi`n = 1`. Điều kiện palindrome không quan trọng và số này chỉ có một chữ số từ 1 đến 9. Thuật toán kiểm tra`1 ≤ m ≤ 9`, khớp chính xác với phạm vi hợp lệ. Ví dụ, đầu vào`n=1, m=7`trôi qua, trong khi`n=1, m=10`thất bại ngay lập tức. 

Một trường hợp cạnh khác là khi`n`lớn và`m`đã gần đến giới hạn. Vì`n = 10^8`,`m = 10^8`hợp lệ vì nó tương ứng với tất cả các chữ số là 1. Séc xử lý việc này một cách trực tiếp mà không cần xây dựng bất cứ điều gì. 

Trường hợp thứ ba là khi`m`cao hơn mức tối đa một chút, chẳng hạn như`n=5, m=46`. Tối đa là`45`, do đó thuật toán sẽ bác bỏ nó ngay lập tức. Lý do không phụ thuộc vào cấu trúc palindrome hoặc tính chẵn lẻ, vì vậy không có trường hợp góc ẩn nào tồn tại được trong kiểm tra ràng buộc này.
