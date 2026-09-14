---
title: "CF 104679A - Năm thứ nhất, Năm thứ hai"
description: "Chúng ta được cho hai số tóm tắt một cặp số nguyên dương chưa biết. Một số biểu thị tổng của chúng và số kia biểu thị hiệu của chúng, trong đó hiệu được lấy bằng thứ nhất trừ thứ hai. Từ hai giá trị này, chúng ta cần xây dựng lại cặp ban đầu."
date: "2026-06-29T09:00:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104679
codeforces_index: "A"
codeforces_contest_name: "Replay of Battle of Brains 2022, University of Dhaka"
rating: 0
weight: 104679
solve_time_s: 48
verified: true
draft: false
---

[CF 104679A - Năm thứ nhất, Năm thứ hai](https://codeforces.com/problemset/problem/104679/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 48s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho hai số tóm tắt một cặp số nguyên dương chưa biết. Một số biểu thị tổng của chúng và số kia biểu thị hiệu của chúng, trong đó hiệu được lấy bằng thứ nhất trừ thứ hai. Từ hai giá trị này, chúng ta cần xây dựng lại cặp ban đầu. 

Nhiệm vụ cơ bản là đảo ngược một hệ thống tuyến tính đơn giản. Thay vì được cung cấp trực tiếp hai số nguyên chưa biết, chúng ta được cung cấp hai phép đo kết hợp của chúng và phải khôi phục các giá trị ban đầu một cách duy nhất. 

Các ràng buộc có cấu trúc tối thiểu vì việc tính toán hoàn toàn là số học. Điều này ngụ ý rằng giải pháp phải chạy trong thời gian không đổi cho mỗi trường hợp thử nghiệm, vì bất kỳ vòng lặp nào trên phạm vi lớn sẽ tốn chi phí không cần thiết. Do đó, cách tiếp cận dự kiến ​​sẽ là thao tác đại số trực tiếp. 

Một vấn đề tế nhị phát sinh từ tính hợp lệ của các giá trị được xây dựng lại. Vì các số nguyên ban đầu là dương nên kết quả tính toán cũng phải là số nguyên dương. Một trường hợp cạnh quan trọng khác là tính chẵn lẻ: nếu tổng và hiệu không tạo ra kết quả số nguyên khi chia cho 2 thì đầu vào không nhất quán với bất kỳ cặp số nguyên hợp lệ nào. 

Ví dụ: nếu đầu vào là tổng 5 và chênh lệch 2 thì việc xây dựng lại sẽ hoạt động rõ ràng. Nhưng nếu đầu vào là tổng bằng 5 và chênh lệch là 3 thì cả hai giá trị được xây dựng lại đều trở thành không phải số nguyên, điều này báo hiệu tính không hợp lệ trong cài đặt số nguyên nghiêm ngặt. 

## Phương pháp tiếp cận 

Một cách diễn giải thô bạo sẽ thử tất cả các cặp số nguyên dương có thể có và kiểm tra xem cặp nào khớp với cả tổng và hiệu đã cho. Đối với mỗi ứng cử viên x và y, chúng tôi sẽ xác minh xem x + y có bằng tổng đã cho hay không và x − y có bằng hiệu đã cho hay không. Điều này hoạt động về mặt khái niệm vì nó tìm kiếm triệt để không gian giải pháp. 

Tuy nhiên, số lượng cặp ứng cử viên tăng theo phương trình bậc hai theo độ lớn của tổng. Nếu tổng theo thứ tự 10^9, thì số cặp cần kiểm tra tỷ lệ thuận với bình phương độ lớn đó, điều này không khả thi về mặt tính toán. 

Quan sát quan trọng là hai phương trình đã xác định một hệ thống tuyến tính. Thay vì tìm kiếm, chúng ta có thể giải trực tiếp những ẩn số bằng cách loại bỏ các biến. Việc cộng cả hai phương trình sẽ cô lập một biến và trừ chúng sẽ cô lập biến kia. Điều này làm giảm vấn đề từ tìm kiếm sang số học theo thời gian không đổi. 

Cấu trúc của vấn đề là yếu tố tạo nên sự giảm bớt này. Các ràng buộc không đưa ra bất kỳ hành vi phi tuyến nào hoặc các điều kiện bổ sung có thể làm phức tạp phép nghịch đảo, do đó đại số trực tiếp xác định đầy đủ lời giải. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(S²) | O(1) | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc hai giá trị đã cho, biểu thị tổng của các số chưa biết và hiệu của chúng theo một thứ tự cố định. 

Thứ tự quan trọng vì sự khác biệt xác định biến nào lớn hơn. 
2. Sử dụng mối quan hệ giữa hai phương trình để tách số đầu tiên. Việc cộng tổng và hiệu sẽ hủy biến thứ hai, để lại gấp đôi số thứ nhất. 
3. Chia kết quả cho hai để được số ban đầu đầu tiên. Phép chia này được đảm bảo chỉ có giá trị nếu đầu vào phù hợp với nghiệm số nguyên. 
4. Tính số thứ hai bằng cách lấy tổng trừ số thứ nhất, hoặc tương đương bằng cách lấy tổng trừ đi hiệu rồi chia cho hai. 
5. Xuất cả hai giá trị được xây dựng lại. 

Tại sao nó hoạt động: hệ thống xác định một điểm giao nhau tuyến tính duy nhất trong hai chiều. Phép biến đổi từ các biến ban đầu thành tổng và hiệu là không thể nghịch đảo miễn là phép chia số học cho hai thu được số nguyên. Vì phép cộng và phép trừ là các phép toán tuyến tính nên việc đảo ngược chúng sẽ duy trì tính nhất quán và đảm bảo rằng cặp được xây dựng lại thỏa mãn chính xác cả hai phương trình ban đầu.

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    data = input().split()
    S = int(data[0])
    D = int(data[1])

    x = (S + D) // 2
    y = (S - D) // 2

    print(x, y)

if __name__ == "__main__":
    solve()
```Giải pháp đọc hai giá trị đầu vào và áp dụng ngay phép nghịch đảo đại số của hệ thống. Biểu thức (S + D) tương ứng với hai lần số thứ nhất, trong khi (S − D) tương ứng với hai lần số thứ hai. Phép chia số nguyên được sử dụng vì bài toán đảm bảo đầu vào hợp lệ khi việc tái cấu trúc là chính xác. 

Một lỗi phổ biến là đảo ngược dấu hoặc hoán đổi công thức, điều này tạo ra các cặp trông đúng nhưng không chính xác. Một vấn đề tinh vi khác là quên rằng cả hai giá trị đều phải là số nguyên, nhưng trong cài đặt lập trình cạnh tranh tiêu chuẩn cho vấn đề này, đầu vào được xây dựng để tránh kết quả phân số. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào: 

S = 10, D = 4 

Chúng tôi tính toán các giá trị được xây dựng lại từng bước. 

| Bước | Biểu hiện | Giá trị | 
| --- | --- | --- | 
| Tổng hợp | S | 10 | 
| Sự khác biệt | D | 4 | 
| Số đầu tiên | (S + D) / 2 | 7 | 
| Số thứ hai | (S − D) / 2 | 3 | 

Điều này xác nhận rằng 7 + 3 = 10 và 7 − 3 = 4, do đó việc tái thiết là nhất quán. Dấu vết cho thấy phép biến đổi tách biệt rõ ràng hai ẩn số mà không có sự mơ hồ. 

### Ví dụ 2 

đầu vào: 

S = 8, D = 2 

| Bước | Biểu hiện | Giá trị | 
| --- | --- | --- | 
| Tổng hợp | S | 8 | 
| Sự khác biệt | D | 2 | 
| Số đầu tiên | (S + D) / 2 | 5 | 
| Số thứ hai | (S − D) / 2 | 3 | 

Ở đây cặp được xây dựng lại là 5 và 3. Việc xác minh sự thay thế ngược lại xác nhận tính đúng đắn. Ví dụ này củng cố rằng quy trình này có tính đối xứng và không phụ thuộc vào tỷ lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ một số phép tính số học cố định được thực hiện | 
| Không gian | O(1) | Không có cấu trúc dữ liệu bổ sung nào được sử dụng | 

Quá trình tính toán không đổi bất kể kích thước đầu vào, dễ dàng phù hợp với giới hạn lập trình cạnh tranh điển hình mà ngay cả những lô trường hợp thử nghiệm lớn cũng không ảnh hưởng đến hiệu suất. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

def solve():
    import sys
    input = sys.stdin.readline
    S, D = map(int, input().split())
    x = (S + D) // 2
    y = (S - D) // 2
    print(x, y)

assert run("10 4\n") == "7 3", "sample 1"
assert run("8 2\n") == "5 3", "sample 2"

assert run("2 0\n") == "1 1", "minimum equal numbers"
assert run("1000000000 0\n") == "500000000 500000000", "large equal case"
assert run("9 1\n") == "5 4", "odd sum with valid reconstruction"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 0 | 1 1 | Trường hợp đối xứng tối thiểu | 
| 1000000000 0 | 500000000 500000000 | Độ ổn định giá trị lớn | 
| 9 1 | 5 4 | Tái thiết tổng số lẻ chung | 

## Vỏ cạnh 

Trường hợp một cạnh là khi chênh lệch bằng 0. Đối với đầu vào 2 và 0, việc tái cấu trúc mang lại cả hai giá trị bằng nhau. Thuật toán tính toán (2 + 0) / 2 = 1 và (2 − 0) / 2 = 1, tạo ra một cặp giống hệt nhau hợp lệ. Điều này xác nhận rằng phương pháp này xử lý tính đối xứng một cách tự nhiên mà không cần phân nhánh đặc biệt. 

Một trường hợp khác là khi tổng lớn nhưng chênh lệch cũng lớn và gần với tổng. Đối với đầu vào 100 và 98, phép tính mang lại x = 99 và y = 1. Số học vẫn ổn định vì cả hai biểu thức vẫn tạo ra số nguyên không âm và phép trừ không gây ra các vấn đề tràn hoặc độ chính xác trong Python. 

Trường hợp cuối cùng là tính nhất quán chẵn lẻ. Nếu giá trị đầu vào giả định như 5 và 2 được cho phép thì công thức sẽ tạo ra các giá trị phân số. Thuật toán giả định các đầu vào được xây dựng sao cho cả (S + D) và (S − D) đều chẵn, đảm bảo đầu ra là số nguyên.
