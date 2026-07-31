---
title: "CF 102569K - Bảng"
description: "Chúng ta có bốn thanh sẽ trở thành bốn chân bàn. Chiều dài của chúng xác định chiều cao của bốn góc của mặt bàn vì mỗi chân dài từ sàn đến bề mặt."
date: "2026-07-31T07:58:33+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102569
codeforces_index: "K"
codeforces_contest_name: "2020, XIII Samara Regional Intercollegiate Programming Contest"
rating: 0
weight: 102569
solve_time_s: 158
verified: true
draft: false
---

[CF 102569K - Bảng](https://codeforces.com/problemset/problem/102569/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2 phút 38 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có bốn thanh sẽ trở thành bốn chân bàn. Chiều dài của chúng xác định chiều cao của bốn góc của mặt bàn vì mỗi chân dài từ sàn đến bề mặt. Câu hỏi đặt ra là liệu chúng ta có thể đặt bốn chiều cao này ở các góc của một hình chữ nhật nào đó sao cho một mặt bàn phẳng đi qua tất cả bốn điểm cuối hay không. 

Một mặt phẳng trên một hình chữ nhật có cấu trúc đặc biệt. Nếu chúng ta nhìn vào bốn góc, chiều cao thay đổi độc lập dọc theo hai hướng hình chữ nhật. Điều này có nghĩa là chiều cao của bốn góc không thể tùy ý. Nhiệm vụ là quyết định xem bốn số đã cho có thể được sắp xếp để thỏa mãn điều kiện hình học đó hay không. Đầu ra là`YES`nếu có sự sắp xếp như vậy và`NO`nếu không thì. 

Mỗi thanh có chiều dài có thể lớn bằng$10^9$, nhưng chỉ có bốn thanh. Kích thước của các giá trị loại trừ các phương pháp phụ thuộc vào việc thử độ dài có thể hoặc sử dụng mô phỏng dựa trên tọa độ. Vì số phần tử là cố định nên lời giải phải dựa vào các tính chất toán học của bốn giá trị thay vì tìm kiếm trong một không gian trạng thái rộng lớn. Việc sắp xếp hoặc kiểm tra số lượng sắp xếp không đổi dễ dàng nằm trong giới hạn. 

Các trường hợp cạnh chính xuất phát từ việc giả định rằng tất cả các thanh phải có cùng độ dài hoặc chỉ các cặp bằng nhau mới hoạt động. Việc triển khai bất cẩn có thể bỏ sót các bảng dốc hợp lệ hoặc chấp nhận các tập hợp không hợp lệ. 

Ví dụ, đầu vào`1 3 2 2`nên sản xuất`YES`. Bốn chiều cao có thể được đặt sao cho các góc đối diện có tổng bằng nhau:$1+3=2+2$. Một giải pháp chỉ chấp nhận bốn giá trị bằng nhau hoặc hai cặp giá trị bằng nhau sẽ từ chối trường hợp này một cách không chính xác. 

Một trường hợp khác là`1 2 3 10`, cái nào sẽ tạo ra`NO`. Tổng giá trị nhỏ nhất và lớn nhất bằng$11$, trong khi các giá trị ở giữa cộng lại thành$5$. Không có vị trí nào có thể làm cho tổng các góc đối diện khớp nhau, do đó không có mặt phẳng nào có thể đi qua cả bốn điểm cuối. 

Vụ án`5 5 5 5`nên sản xuất`YES`. Mặt bàn phẳng chỉ đơn giản là một trường hợp đặc biệt của mặt bàn nghiêng, do đó việc yêu cầu độ dốc khác 0 sẽ từ chối đầu vào này một cách không chính xác. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là thử mọi cách gán bốn thanh cho bốn góc hình chữ nhật. chỉ có$4! = 24$hoán vị. Đối với mỗi cách sắp xếp, chúng ta có thể kiểm tra xem các góc đối diện có tổng chiều cao bằng nhau hay không. Điều này có tác dụng vì mọi vị trí hình chữ nhật hợp lệ đều tương ứng với một trong các phép gán này, vì vậy, việc thử tất cả các vị trí đó không thể bỏ sót một giải pháp nào. 

Cách tiếp cận bạo lực đã đủ nhanh vì 24 lần kiểm tra là một khối lượng công việc không đổi. Tuy nhiên, nó ẩn cấu trúc cơ bản. Quan sát thực tế là chiều cao bốn góc của bất kỳ mặt phẳng nào trên hình chữ nhật luôn thỏa mãn một đẳng thức đơn giản. 

Giả sử các góc hình chữ nhật có chiều cao$x_1, x_2, x_3, x_4$, nơi các góc đối diện được ghép nối. Di chuyển qua một hướng hình chữ nhật sẽ thêm cùng một lượng vào chiều cao và di chuyển qua hướng khác cũng thêm một lượng tương tự. Bốn giá trị có thể được biểu diễn dưới dạng:$$c,\quad c+a,\quad c+b,\quad c+a+b$$Tổng giá trị đầu tiên và cuối cùng là:$$c + (c+a+b) = 2c+a+b$$Tổng của hai giá trị còn lại là:$$(c+a)+(c+b)=2c+a+b$$Vậy tổng hai góc đối diện phải bằng nhau. Sau khi sắp xếp bốn giá trị như$a \leq b \leq c \leq d$, khả năng ghép nối duy nhất của các góc đối diện là góc nhỏ nhất có giá trị lớn nhất và hai giá trị ở giữa cùng nhau. Điều kiện trở thành:$$a+d=b+c$$Điều này đưa ra giải pháp trực tiếp về thời gian không đổi sau khi sắp xếp. 

Phương pháp brute-force hoạt động vì không gian tìm kiếm chỉ chứa 24 bố cục có thể có, nhưng phương trình trên nén tất cả những kiểm tra đó thành một so sánh. Ràng buộc hình học biến thành một thuộc tính số học của các giá trị được sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(4!) | O(1) | Đã chấp nhận nhưng tìm kiếm không cần thiết | 
| Tối ưu | O(4 log 4) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc độ dài của bốn vạch và sắp xếp chúng. Đặt các giá trị được sắp xếp là$a, b, c, d$. Việc sắp xếp loại bỏ nhu cầu suy nghĩ về tất cả các vị trí có thể có vì giá trị nhỏ nhất và lớn nhất phải tạo thành một cặp đối diện trong bất kỳ cách sắp xếp hợp lệ nào. 
2. Kiểm tra xem$a+d=b+c$. Điều này so sánh hai tổng góc đối diện có thể có sau khi sắp xếp các giá trị. Nếu chúng bằng nhau thì bốn chiều cao có thể được sắp xếp thành các góc của một hình chữ nhật với một mặt phẳng tiếp xúc với tất cả chúng. 
3. In`YES`khi đẳng thức giữ nguyên và`NO`nếu không thì. Sự đẳng thức vừa cần vừa đủ nên không cần có hình học bổ sung nào để xác minh. 

Tại sao nó hoạt động: 

Đối với bất kỳ mặt phẳng nào đặt trên hình chữ nhật, chiều cao các góc có dạng$c, c+a, c+b, c+a+b$. Các góc đối diện luôn có tổng bằng nhau nên mọi bảng hợp lệ phải thỏa mãn đẳng thức được sắp xếp$a+d=b+c$. Ngược lại, nếu đẳng thức giữ nguyên, hãy chọn các giá trị đã sắp xếp là$a,b,c,d$đặt xung quanh hình chữ nhật theo thứ tự đó. Sự khác biệt giữa các góc liền kề xác định những thay đổi nhất quán dọc theo hai hướng hình chữ nhật, do đó một mặt phẳng tồn tại qua cả bốn điểm. Điều kiện mô tả chính xác các bảng hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    a = list(map(int, input().split()))
    a.sort()

    if a[0] + a[3] == a[1] + a[2]:
        print("YES")
    else:
        print("NO")

if __name__ == "__main__":
    solve()
```Giải pháp đọc bốn chiều cao và sắp xếp chúng để có thể kiểm tra mối quan hệ hình học mà không cần xem xét các vị trí riêng lẻ. Việc so sánh sử dụng các giá trị nhỏ nhất và lớn nhất ở một bên và hai giá trị ở giữa ở phía bên kia. 

Số nguyên Python xử lý các giá trị lên tới$2 \times 10^9$không có bất kỳ vấn đề tràn nào, mặc dù bản thân các giá trị đầu vào có thể đạt tới$10^9$. Không có mối lo ngại về ranh giới chỉ số vì mảng luôn chứa chính xác bốn số. 

Thứ tự thực hiện các thao tác rất quan trọng vì công thức dựa trên các vị trí được sắp xếp. Việc kiểm tra thứ tự ban đầu sẽ chỉ kiểm tra một vị trí tùy ý và có thể từ chối sự sắp xếp hợp lệ. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên, đầu vào là`1 1 1 1`. 

| Giá trị được sắp xếp | Bên trái$a+d$| Bên phải$b+c$| Quyết định | 
| --- | --- | --- | --- | 
| 1, 1, 1, 1 | 2 | 2 | CÓ | 

Cả bốn góc đều có cùng chiều cao nên mặt bàn phẳng. Sự đẳng thức được giữ vì cả hai tổng ở góc đối diện đều giống nhau. 

Đối với mẫu thứ hai, đầu vào là`1 5 1 5`. 

| Giá trị được sắp xếp | Bên trái$a+d$| Bên phải$b+c$| Quyết định | 
| --- | --- | --- | --- | 
| 1, 1, 5, 5 | 6 | 6 | CÓ | 

Hai chân ngắn hơn có thể chiếm các góc đối diện, và hai chân cao hơn có thể chiếm các góc đối diện khác. Các tổng bằng nhau cho phép bề mặt là một mặt phẳng nghiêng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Việc sắp xếp bốn số cần một lượng công việc cố định | 
| Không gian | O(1) | Chỉ có bốn số nguyên được lưu trữ | 

Các ràng buộc dễ dàng được thỏa mãn vì thuật toán không phụ thuộc vào kích thước của các giá trị. Nó chỉ thực hiện một số phép tính số học sau khi sắp xếp một mảng có kích thước cố định. 

## Trường hợp thử nghiệm```python
import sys
import io

def solution(inp: str) -> str:
    data = list(map(int, inp.split()))
    data.sort()
    return "YES\n" if data[0] + data[3] == data[1] + data[2] else "NO\n"

def run(inp: str) -> str:
    return solution(inp)

assert run("1 1 1 1") == "YES\n", "sample 1"
assert run("1 5 1 5") == "YES\n", "sample 2"
assert run("1 3 2 2") == "YES\n", "sample 3"

assert run("1 2 3 10") == "NO\n", "invalid opposite sums"
assert run("1000000000 1000000000 1000000000 1000000000") == "YES\n", "maximum equal values"
assert run("1 1 1 2") == "NO\n", "single different leg"
assert run("1 2 2 3") == "YES\n", "minimum non-flat slope"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 2 3 10`|`NO`| Từ chối các trường hợp ghép góc đối diện không hoạt động | 
|`1000000000 1000000000 1000000000 1000000000`|`YES`| Xử lý các giá trị tối đa và bảng phẳng | 
|`1 1 1 2`|`NO`| Kiểm tra xem một chiều cao chưa từng có là chưa đủ | 
|`1 2 2 3`|`YES`| Xác nhận các bề mặt dốc hợp lệ với các giá trị ở giữa lặp lại | 

## Vỏ cạnh 

đầu vào`1 3 2 2`chứng tỏ tại sao không cần phải có các cặp bằng nhau. Sau khi sắp xếp, các giá trị trở thành`1,2,2,3`. Thuật toán kiểm tra$1+3=2+2$, điều này đúng nên nó trả về`YES`. Mặt bàn tương ứng bị nghiêng vì chiều cao không bằng nhau. 

đầu vào`1 2 3 10`chứng tỏ sự sắp xếp không hợp lệ. Sắp xếp mang lại`1,2,3,10`và thuật toán kiểm tra$1+10=2+3$. Từ`11`không bằng`5`, mặt phẳng cần tìm không thể tồn tại, vì vậy câu trả lời là`NO`. 

đầu vào`5 5 5 5`minh họa trường hợp bàn phẳng. Việc sắp xếp giữ nguyên các giá trị và cả hai vế của phương trình đều bằng nhau`10`. Thuật toán chấp nhận nó vì bề mặt nằm ngang chỉ đơn giản là một mặt phẳng có độ dốc bằng 0.
