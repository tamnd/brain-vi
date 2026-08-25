---
title: "CF 102219B - SpongeBob SquarePants"
description: "Mỗi trường hợp thử nghiệm mô tả một hình bốn cạnh sử dụng chiều rộng w và chiều cao h của nó. Vì tất cả các góc đều là góc vuông nên câu hỏi duy nhất để phân biệt hình vuông với hình chữ nhật thông thường là liệu độ dài hai cạnh có bằng nhau hay không."
date: "2026-08-25T19:06:32+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102219
codeforces_index: "B"
codeforces_contest_name: "2019 ICPC Malaysia National"
rating: 0
weight: 102219
solve_time_s: 3729
verified: true
draft: false
---

[CF 102219B - SpongeBob SquarePants](https://codeforces.com/problemset/problem/102219/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1h 2p 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi trường hợp thử nghiệm mô tả một hình dạng bốn cạnh bằng chiều rộng của nó`w`và chiều cao`h`. Vì tất cả các góc đều là góc vuông nên câu hỏi duy nhất để phân biệt hình vuông với hình chữ nhật thông thường là liệu độ dài hai cạnh có bằng nhau hay không. 

Đối với một hình dạng cụ thể, đầu ra yêu cầu là`YES`khi`w == h`, vì chiều rộng và chiều cao bằng nhau làm cho hình chữ nhật trở thành hình vuông. Khi hai giá trị khác nhau, đầu ra là`NO`. 

Kích thước là số nguyên dương giữa`1`Và`1,000,000`. Giới hạn trên đủ nhỏ nên việc so sánh một số nguyên là không đáng kể, nhưng nó cũng cho chúng ta biết rằng không có lý do gì để thực hiện mô phỏng hình học hoặc liệt kê các độ dài cạnh có thể có. Số lượng trường hợp thử nghiệm được xử lý độc lập, do đó mục tiêu hữu ích là công việc không đổi trên mỗi trường hợp, mang lại thời gian tuyến tính cho số lượng trường hợp. Ngay cả khi có nhiều trường hợp thử nghiệm,`O(T)`giải pháp chỉ thực hiện một so sánh cho mỗi hình dạng, trong khi phương pháp thực hiện tới một triệu thao tác cho mỗi hình dạng có thể nhanh chóng trở nên quá đắt nếu dưới giới hạn một giây. 

Trường hợp cạnh đầu tiên là hình vuông nhỏ nhất có thể. Đối với đầu vào`1 1`, đầu ra đúng là`YES`. Việc triển khai bất cẩn để kiểm tra xem cả hai chiều có lớn hơn một hay không sẽ từ chối nó một cách không chính xác, mặc dù hình vuông có thể có độ dài cạnh bằng một. 

Một trường hợp cạnh khác là một hình chữ nhật có kích thước chỉ khác nhau một. Đối với đầu vào`7 8`, đầu ra là`NO`. Mã vô tình sử dụng một điều kiện như`abs(w - h) <= 1`sẽ chấp nhận hình dạng này một cách không chính xác. Sự bình đẳng phải chính xác. 

Hướng của các kích thước không thay đổi cho dù hình dạng là hình vuông. Đối với đầu vào`3 10`, câu trả lời là`NO`, và điều này cũng đúng với`10 3`. Một giải pháp xử lý chiều rộng và chiều cao khác nhau ngoài việc so sánh chúng có thể tạo ra sự phụ thuộc vào hướng không cần thiết. 

Cuối cùng, các giá trị tối đa phải hoạt động bình thường. Đối với đầu vào`1000000 1000000`, câu trả lời là`YES`. Các giá trị phù hợp thoải mái với số nguyên Python, do đó không cần xử lý số đặc biệt. 

## Phương pháp tiếp cận 

Một cách mạnh mẽ để giải quyết vấn đề là thử mọi độ dài cạnh có thể từ`1`bởi vì`max(w, h)`và hỏi liệu ứng cử viên đó có thể là độ dài cạnh chung của hình đó hay không. Nếu ứng viên bằng cả hai chiều thì hình đó là hình vuông. Nếu tất cả các ứng cử viên đều kiệt sức thì không phải vậy. 

Phương pháp này đúng vì hình vuông có kích thước`w`Và`h`có đúng một độ dài cạnh chung có thể có, đó là`w = h`. Tuy nhiên, nó có thể hoạt động tới`1,000,000`ứng viên kiểm tra một trường hợp kiểm thử duy nhất. Sang`T`trường hợp, công việc trong trường hợp xấu nhất là`1,000,000T`séc. Bài toán không đưa ra lý do hữu ích để tốn nhiều thời gian như vậy khi câu trả lời đã được mã hóa trực tiếp trong hai giá trị đầu vào. 

Quan sát quan trọng là không có hình học ẩn nào để tái tạo lại. Một hình có bốn cạnh có bốn góc vuông là hình vuông khi chiều rộng và chiều cao của nó bằng nhau. Cách tiếp cận bạo lực có hiệu quả vì cuối cùng nó kiểm tra sự bình đẳng một cách gián tiếp, nhưng quan sát rằng bản thân sự bình đẳng là điều kiện hoàn chỉnh cho phép chúng ta thay thế tới một triệu kiểm tra bằng một so sánh. 

Thuật toán kết quả đọc từng cặp`(w, h)`, so sánh hai số nguyên và in`YES`nếu chúng bằng nhau và`NO`nếu không thì. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(T * max(w, h))`, lên tới`1,000,000T`séc |`O(1)`| Quá chậm | 
| Tối ưu |`O(T)`|`O(1)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`T`. Mỗi trường hợp thử nghiệm đại diện cho một hình dạng độc lập, do đó không cần chia sẻ thông tin giữa các trường hợp. 
2. Với mỗi test case, hãy đọc`w`Và`h`. Đây là độ dài hai cạnh xác định xem hình chữ nhật có phải là hình vuông hay không. 
3. So sánh`w`Và`h`. Nếu như`w == h`, cả hai kích thước đều có cùng độ dài, đó chính xác là điều kiện xác định cần thiết ở đây để hình chữ nhật là hình vuông. 
4. In`YES`khi sự so sánh là đúng và`NO`nếu không thì. Không cần tính toán hình học nào khác vì dữ liệu đầu vào đã đảm bảo rằng hình có bốn góc vuông. 

### Tại sao nó hoạt động 

Đối với mỗi hình được xử lý, thuật toán sẽ kiểm tra điều kiện cần và đủ để là hình vuông. Nếu như`w == h`, hình chữ nhật có chiều rộng và chiều cao bằng nhau nên là hình vuông và thuật toán in ra`YES`. Nếu như`w != h`, hai cạnh của nó khác nhau nên không thể là hình vuông và thuật toán in ra`NO`. Vì mọi trường hợp thử nghiệm đều được quyết định trực tiếp từ điều kiện chính xác này nên thuật toán không thể tạo ra sự phân loại không chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())

    for _ in range(t):
        w, h = map(int, input().split())

        if w == h:
            print("YES")
        else:
            print("NO")

if __name__ == "__main__":
    solve()
```Dòng đầu tiên được đọc là`t`, điều khiển chính xác số lượng cặp thứ nguyên được xử lý. Điều này khớp với định dạng đầu vào và ngăn chương trình vô tình đọc vượt quá các trường hợp kiểm thử. 

Bên trong vòng lặp,`w`Và`h`được phân tích cú pháp dưới dạng số nguyên. Quyết định duy nhất là kiểm tra sự bình đẳng`w == h`, trực tiếp triển khai thuộc tính xác định của hình vuông từ hướng dẫn thuật toán. 

Mã không cần điều kiện biên đặc biệt cho`1`hoặc`1,000,000`. Cả hai đều là giá trị số nguyên thông thường và Python xử lý chúng mà không bị tràn. Cũng không có vấn đề riêng lẻ nào vì thuật toán không lặp lại theo các thứ nguyên hoặc phạm vi sử dụng. Mỗi trường hợp thử nghiệm yêu cầu chính xác một so sánh và một đầu ra. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Mẫu chứa bốn hình dạng độc lập. Trạng thái của các biến chính và kết quả so sánh là: 

| Trường hợp thử nghiệm |`w`|`h`|`w == h`| Đầu ra | 
| --- | --- | --- | --- | --- | 
| 1 | 9 | 9 |`True`|`YES`| 
| 2 | 16 | 30 |`False`|`NO`| 
| 3 | 200 | 33 |`False`|`NO`| 
| 4 | 547 | 547 |`True`|`YES`| 

Hình thứ nhất và thứ tư có kích thước bằng nhau nên cả hai đều được chấp nhận là quần vuông. Hai cái ở giữa có kích thước khác nhau và bị từ chối. Mỗi quyết định chỉ phụ thuộc vào cặp hiện tại, điều này xác nhận rằng các trường hợp thử nghiệm không yêu cầu bất kỳ trạng thái chia sẻ nào. 

### Ví dụ bổ sung 

Hãy xem xét đầu vào:```
5
1 1
1 2
7 8
1000000 999999
1000000 1000000
```Việc thực hiện là: 

| Trường hợp thử nghiệm |`w`|`h`|`w == h`| Đầu ra | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1 |`True`|`YES`| 
| 2 | 1 | 2 |`False`|`NO`| 
| 3 | 7 | 8 |`False`|`NO`| 
| 4 | 1000000 | 999999 |`False`|`NO`| 
| 5 | 1000000 | 1000000 |`True`|`YES`| 

Dấu vết này bao gồm cả ranh giới của phạm vi cho phép và cũng kiểm tra một cặp có kích thước chỉ khác nhau một. Thuật toán không bao giờ coi sự gần bằng là bằng nhau, vì vậy`7 8`sản xuất chính xác`NO`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(T)`| Mỗi trong số`T`các trường hợp thử nghiệm yêu cầu một phép so sánh đẳng thức và một thao tác đầu ra. | 
| Không gian |`O(1)`| Ngoài các giá trị đầu vào cho ca kiểm thử hiện tại, thuật toán không lưu trữ dữ liệu nào tỷ lệ với`T`. | 

Kích thước có thể lớn tới một triệu, nhưng độ lớn của chúng không ảnh hưởng đến thời gian chạy vì giải pháp không lặp lại chúng. Ngay cả với số lượng lớn các ca kiểm thử, công việc vẫn tăng trưởng tuyến tính với`T`, đây là độ phức tạp thích hợp cho đầu vào trong đó mọi trường hợp đều phải được đọc và phân loại. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    input = sys.stdin.readline
    t = int(input())
    for _ in range(t):
        w, h = map(int, input().split())
        print("YES" if w == h else "NO")

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided sample
assert run("""4
9 9
16 30
200 33
547 547
""") == """YES
NO
NO
YES
""", "sample 1"

# Minimum-size values
assert run("""3
1 1
1 2
2 1
""") == """YES
NO
NO
""", "minimum-size and orientation cases"

# Maximum-size values
assert run("""3
1000000 1000000
1000000 999999
999999 1000000
""") == """YES
NO
NO
""", "maximum-size cases"

# Equality and near-equality
assert run("""4
7 7
7 8
8 7
8 8
""") == """YES
NO
NO
YES
""", "exact equality versus one-unit difference"

# Multiple independent cases
assert run("""5
3 10
50 50
123456 654321
999999 999999
42 43
""") == """NO
YES
NO
YES
NO
""", "mixed cases")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`,`1 2`,`2 1`|`YES`,`NO`,`NO`| Kích thước tối thiểu và chiều rộng và chiều cao đảo ngược | 
|`1000000 1000000`,`1000000 999999`,`999999 1000000`|`YES`,`NO`,`NO`| Kích thước tối đa và sự khác biệt ranh giới | 
|`7 7`,`7 8`,`8 7`,`8 8`|`YES`,`NO`,`NO`,`YES`| Bình đẳng chính xác và chênh lệch một đơn vị | 
| Đầu vào năm trường hợp hỗn hợp |`NO`,`YES`,`NO`,`YES`,`NO`| Xử lý độc lập nhiều trường hợp | 
| Đầu vào mẫu |`YES`,`NO`,`NO`,`YES`| Hành vi mẫu chính thức | 

## Vỏ cạnh 

Hình vuông có kích thước nhỏ nhất là`1 1`. Thuật toán đọc`w = 1`Và`h = 1`, đánh giá`1 == 1`là đúng và in`YES`. Không có yêu cầu về độ dài cạnh lớn hơn một, vì vậy điều này chấp nhận chính xác hình vuông nhỏ nhất có thể. 

Đối với hình chữ nhật gần bằng nhau`7 8`, thuật toán đánh giá`7 == 8`là sai và in`NO`. Một giải pháp bất cẩn dựa trên sự khác biệt nhỏ có thể phân loại không chính xác đây là hình vuông, nhưng hình vuông yêu cầu đẳng thức chính xác chứ không phải đẳng thức gần đúng. 

Đối với các kích thước đảo ngược như`10 3`, thuật toán đánh giá`10 == 3`là sai và in`NO`. Kết quả tương tự sẽ xảy ra đối với`3 10`. Vì điều kiện hình vuông đối xứng về chiều rộng và chiều cao nên thứ tự của chúng không ảnh hưởng đến việc phân loại. 

Đối với hình vuông tối đa`1000000 1000000`, sự so sánh đánh giá là đúng và tạo ra`YES`. Vì`1000000 999999`, nó đánh giá là sai và tạo ra`NO`. Những trường hợp này xác nhận rằng ranh giới trên được xử lý trực tiếp mà không bị tràn, giới hạn lặp hoặc các trường hợp đặc biệt.
