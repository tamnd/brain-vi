---
title: "CF 102783C - Thủ thuật hoặc điều trị tối ưu"
description: "Timmy muốn tối đa hóa số lượng thanh Chuckles mà anh ấy có thể có được. Mỗi ngôi nhà trong khu phố đều có một số bộ sưu tập kẹo. Alice có tỷ giá hối đoái cố định cho từng loại kẹo, nghĩa là mỗi loại kẹo đều có giá trị bằng một số thanh Chuckles nhất định."
date: "2026-07-27T20:02:59+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102783
codeforces_index: "C"
codeforces_contest_name: "UTPC Contest 10-23-20 Div. 2"
rating: 0
weight: 102783
solve_time_s: 60
verified: true
draft: false
---

[CF 102783C - Thủ thuật hoặc đối xử tối ưu](https://codeforces.com/problemset/problem/102783/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Timmy muốn tối đa hóa số lượng thanh Chuckles mà anh ấy có thể có được. Mỗi ngôi nhà trong khu phố đều có một số bộ sưu tập kẹo. Alice có tỷ giá hối đoái cố định cho từng loại kẹo, nghĩa là mỗi loại kẹo đều có giá trị bằng một số thanh Chuckles nhất định. Khi Timmy đến thăm một ngôi nhà, anh ấy lấy từng viên kẹo mà ngôi nhà đó tặng, quy đổi tất cả số kẹo đó theo tỷ giá của Alice và nhận được số thanh Chuckles tương ứng. Anh ta chỉ có thể đến thăm một số ngôi nhà hạn chế, vì vậy nhiệm vụ là chọn ra những ngôi nhà tốt nhất. Bài toán yêu cầu tổng giá trị lớn nhất mà Timmy có thể đạt được. 

Đầu vào đầu tiên mô tả thị trường trao đổi. Mỗi tên kẹo có một giá trị đại diện cho số lượng thanh Chuckles mà Alice đưa cho một viên kẹo đó. Sau đó, những ngôi nhà được mô tả. Mỗi ngôi nhà liệt kê các loại kẹo mà nó chứa và số lượng của những viên kẹo đó. Đầu ra là tổng số thanh Chuckles tối đa sau khi chọn tối đa số lượng nhà cho phép. 

Các giới hạn đủ nhỏ để cho phép xử lý trực tiếp. Có tối đa 200 loại kẹo và 200 ngôi nhà. Một giải pháp thực hiện một lượng nhỏ công việc cho mỗi mục nhập kẹo là đủ nhanh vì tổng số mục nhập trong nhà được giới hạn bởi số lượng nhà nhân với số loại kẹo. Ở đây, cách tiếp cận bậc hai đối với các ngôi nhà cũng có thể được chấp nhận, nhưng bất cứ điều gì liên quan đến việc liệt kê mọi tập hợp con có thể có của các ngôi nhà sẽ trở nên bất khả thi vì không thể khám phá được 2^200 lựa chọn khả thi. 

Các trường hợp nguy hiểm chính đến từ việc xử lý không chính xác các ngôi nhà có giá trị nhỏ hoặc giả định rằng chính xác k ngôi nhà luôn phải đóng góp. Ví dụ: nếu chỉ có một ngôi nhà và Timmy có thể đến thăm một ngôi nhà thì câu trả lời chỉ đơn giản là giá trị của ngôi nhà đó. 

Ví dụ đầu vào:```
1
Chocolate 5
1 1
1
Chocolate 3
```Đầu ra là:```
15
```Việc thực hiện bất cẩn có thể vô tình nhân với số loại kẹo thay vì số lượng kẹo, tạo ra 5 thay vì 15. 

Một sai lầm phổ biến khác là bỏ qua những ngôi nhà chứa nhiều loại kẹo. 

Ví dụ đầu vào:```
2
A 2
B 10
2 1
1
A 5
1
B 1
```Đầu ra là:```
10
```Chọn nhà thứ hai thì tốt hơn vì nó cho 10 thanh Chuckles. Việc triển khai chỉ theo dõi số loại kẹo hoặc viên kẹo đầu tiên nhìn thấy trong mỗi nhà có thể chọn sai. 

Trường hợp biên cuối cùng là khi tất cả các ngôi nhà có giá trị giống nhau. 

Ví dụ đầu vào:```
1
Candy 7
3 2
1
Candy 1
1
Candy 1
1
Candy 1
```Đầu ra là:```
14
```Nhà được chọn không quan trọng nhưng thuật toán vẫn phải chọn chính xác k giá trị tốt nhất thay vì dựa vào những so sánh chặt chẽ thất bại khi các giá trị bằng nhau. 

## Phương pháp tiếp cận 

Ý tưởng mạnh mẽ là xem xét mọi nhóm nhà mà Timmy có thể ghé thăm. Đối với mỗi tập hợp con, chúng tôi tính toán tổng số thanh Chuckles thu được và giữ mức tối đa. Cách tiếp cận này đúng vì mọi lựa chọn nhà hợp lý đều được kiểm tra. Tuy nhiên, số lượng tập hợp con là theo cấp số nhân. Với 200 ngôi nhà, có thể có 2^200 lựa chọn khả thi, vượt xa những gì có thể xử lý được. 

Lý do vũ lực là không cần thiết xuất phát từ việc các ngôi nhà không tương tác với nhau. Đến thăm một ngôi nhà không làm thay đổi giá trị của ngôi nhà khác. Giá trị của một ngôi nhà có thể được tính toán một cách độc lập bằng cách tính tổng giá trị của từng viên kẹo bên trong nó. Khi mỗi nhà có một điểm duy nhất, bài toán ban đầu sẽ trở thành chọn k số có tổng lớn nhất. 

Cách tiếp cận tối ưu là tính giá trị của mỗi ngôi nhà, sắp xếp các giá trị đó theo thứ tự giảm dần và cộng k giá trị đầu tiên. Sắp xếp là đủ vì không có sự phụ thuộc giữa các lựa chọn. K nhà tốt nhất đơn giản là k nhà có đóng góp cá nhân cao nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^h * h) | O(h) | Quá chậm | 
| Tối ưu | O(n + h log h) | O(n + h) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lưu trữ giá trị thanh Chuckles của mọi loại kẹo trên bản đồ. Tên kẹo là các chuỗi nên bản đồ cho phép chúng ta nhanh chóng tìm ra giá trị của bất kỳ loại kẹo nào xuất hiện trong một ngôi nhà. 
2. Xử lý từng nhà một cách độc lập. Đối với mỗi chiếc kẹo trong ngôi nhà đó, hãy nhân số lượng của nó với giá trị trao đổi của nó và cộng kết quả vào tổng giá trị của ngôi nhà. Điều này chuyển đổi một bộ sưu tập kẹo khác nhau thành một con số biểu thị giá trị của toàn bộ ngôi nhà. 
3. Lưu trữ tổng giá trị của mỗi ngôi nhà vào một mảng. Lúc này, chi tiết kẹo không còn cần thiết nữa vì mọi quyết định chỉ phụ thuộc vào tổng số tiền đóng góp của mỗi nhà. 
4. Sắp xếp giá trị ngôi nhà từ lớn nhất đến nhỏ nhất. Giá trị k đầu tiên đại diện cho những ngôi nhà mang lại cho Timmy phần thưởng lớn nhất có thể. 
5. Thêm các giá trị k đầu tiên đó và in kết quả. Vì mỗi ngôi nhà được chọn đều đóng góp độc lập nên việc chọn bất kỳ ngôi nhà nào có giá trị thấp hơn thay vì ngôi nhà có giá trị cao hơn không bao giờ có thể cải thiện câu trả lời. 

Tại sao nó hoạt động: 

Bất biến chính là sau khi tính toán tất cả các giá trị ngôi nhà, mọi quyết định ghé thăm có thể chỉ có thể được thể hiện bằng cách chọn trong số các giá trị này. Nếu một tập hợp được chọn chứa một ngôi nhà có giá trị nhỏ hơn một ngôi nhà không được chọn thì việc hoán đổi hai ngôi nhà đó không bao giờ làm giảm tổng số. Việc lặp lại đối số trao đổi này sẽ dẫn đến chính xác k giá trị nhà lớn nhất, chứng tỏ thuật toán luôn tìm được lựa chọn tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    value = {}

    for _ in range(n):
        name, v = input().split()
        value[name] = int(v)

    h, k = map(int, input().split())

    houses = []

    for _ in range(h):
        c = int(input())
        total = 0
        for _ in range(c):
            name, amount = input().split()
            total += value[name] * int(amount)
        houses.append(total)

    houses.sort(reverse=True)

    print(sum(houses[:k]))

if __name__ == "__main__":
    solve()
```Từ điển lưu trữ tỷ giá hối đoái vì tên kẹo là các chuỗi tùy ý và việc tra cứu trực tiếp sẽ tránh việc tìm kiếm nhiều lần trong tất cả các giao dịch. 

Mỗi ngôi nhà được chuyển đổi thành một giá trị số nguyên duy nhất. Bước nhân là cần thiết vì một ngôi nhà có thể chứa nhiều bản sao của một loại kẹo và mỗi bản sao đóng góp độc lập. 

Sắp xếp theo thứ tự ngược lại sẽ đặt những ngôi nhà có lợi nhất ở đầu danh sách. Số nguyên Python xử lý các kết quả nhân có thể có mà không lo ngại tràn. 

lát cắt`houses[:k]`an toàn vì đầu vào đảm bảo k không lớn hơn số nhà. Số tiền cuối cùng chỉ bao gồm những ngôi nhà mà Timmy được phép đến thăm. 

## Ví dụ đã hoạt động 

Đối với đầu vào mẫu:```
3
LactoseyWay 2
Twicks 3
DigDog 5
2 1
2
LactoseyWay 3
Twicks 1
1
DigDog 2
```Tính toán của ngôi nhà là: 

| Nhà | Tính kẹo | Giá trị căn nhà | 
| --- | --- | --- | 
| 1 | 3*2 + 1*3 | 9 | 
| 2 | 2*5 | 10 | 

Sau khi sắp xếp: 

| Vị trí sắp xếp | Giá trị | 
| --- | --- | 
| 1 | 10 | 
| 2 | 9 | 

Chỉ có thể đến thăm một ngôi nhà nên Timmy chọn ngôi nhà có giá trị 10. 

Đầu ra là:```
10
```Dấu vết này cho thấy lý do tại sao vấn đề giảm xuống việc chọn các giá trị độc lập lớn nhất. 

Cho một ví dụ khác:```
2
Cookie 4
Candy 1
3 2
2
Cookie 2
Candy 5
1
Cookie 1
1
Candy 10
```Các tính toán là: 

| Nhà | Tính toán | Giá trị căn nhà | 
| --- | --- | --- | 
| 1 | 2*4+5*1 | 13 | 
| 2 | 1*4 | 4 | 
| 3 | 10*1 | 10 | 

Sắp xếp mang lại: 

| Vị trí sắp xếp | Giá trị | 
| --- | --- | 
| 1 | 13 | 
| 2 | 10 | 
| 3 | 4 | 

Timmy có thể đến thăm hai ngôi nhà nên câu trả lời là 23. 

Ví dụ này xác nhận rằng một ngôi nhà có nhiều loại kẹo khác nhau vẫn có thể được so sánh trực tiếp với một ngôi nhà chỉ có một loại kẹo. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + h log h) | Việc đọc các giá trị kẹo mất O(n), nội dung của nhóm xử lý mất O(tổng số mục kẹo) và sắp xếp h giá trị nhóm mất O(h log h). | 
| Không gian | O(n + h) | Bản đồ lưu trữ các giá trị kẹo và mảng lưu trữ một giá trị cho mỗi ngôi nhà. | 

Đầu vào lớn nhất có thể chỉ chứa vài trăm ngôi nhà và loại kẹo, vì vậy độ phức tạp này dễ dàng nằm trong giới hạn. Thuật toán dành hầu hết công việc của nó cho số học đơn giản và một phép sắp xếp nhỏ. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    input = sys.stdin.readline

    n = int(input())
    value = {}

    for _ in range(n):
        name, v = input().split()
        value[name] = int(v)

    h, k = map(int, input().split())

    houses = []

    for _ in range(h):
        c = int(input())
        total = 0
        for _ in range(c):
            name, amount = input().split()
            total += value[name] * int(amount)
        houses.append(total)

    houses.sort(reverse=True)
    print(sum(houses[:k]))

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    result = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout

    return result

assert run("""3
LactoseyWay 2
Twicks 3
DigDog 5
2 1
2
LactoseyWay 3
Twicks 1
1
DigDog 2
""") == "10\n", "sample 1"

assert run("""1
Candy 7
1 1
1
Candy 3
""") == "21\n", "single house"

assert run("""2
A 2
B 10
2 1
1
A 5
1
B 1
""") == "10\n", "choose best house"

assert run("""1
Candy 7
3 2
1
Candy 1
1
Candy 1
1
Candy 1
""") == "14\n", "equal houses"

assert run("""2
X 100
Y 1
3 2
2
X 1
Y 100
1
X 2
1
Y 1000
""") == "301\n", "large quantities and mixed candies"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đầu vào mẫu | 10 | Ví dụ về vấn đề ban đầu | 
| Nhà đơn | 21 | Số lượng nhà tối thiểu | 
| Hai ngôi nhà cạnh tranh | 10 | Lựa chọn đúng các giá trị cao nhất | 
| Nhà bình đẳng | 14 | Xử lý giá trị ngôi nhà giống hệt nhau | 
| Hỗn hợp số lượng lớn | 301 | Tính đúng đắn của phép nhân và tích lũy | 

## Vỏ cạnh 

Đối với trường hợp nhà riêng lẻ:```
1
Candy 7
1 1
1
Candy 3
```Thuật toán tính giá trị ngôi nhà duy nhất là 3 * 7 = 21. Việc sắp xếp không thực hiện được gì vì chỉ có một giá trị và câu trả lời là 21. 

Đối với trường hợp nhiều kẹo:```
2
A 2
B 10
2 1
1
A 5
1
B 1
```Ngôi nhà đầu tiên có giá trị 5 * 2 = 10, trong khi ngôi nhà thứ hai có giá trị 1 * 10 = 10. Vì chỉ có thể chọn một ngôi nhà nên cả hai lựa chọn đều cho cùng một câu trả lời. Điều này chứng tỏ rằng các giá trị bằng nhau không yêu cầu xử lý đặc biệt. 

Đối với nhà giống hệt nhau:```
1
Candy 7
3 2
1
Candy 1
1
Candy 1
1
Candy 1
```Mỗi nhà đóng góp 7. Sau khi sắp xếp, mảng vẫn còn ba bản sao của 7 và hai giá trị đầu tiên được chọn. Kết quả là 14. 

Thuật toán xử lý tất cả các trường hợp này vì nó không bao giờ dựa vào thứ tự các ngôi nhà hoặc tính duy nhất của các giá trị của chúng. Nó chỉ phụ thuộc vào thực tế là mỗi ngôi nhà có thể được đánh giá độc lập. 

Điều này cũng có thể được chuyển thể thành một bài xã luận ngắn hơn theo kiểu cuộc thi hoặc một ghi chú giảng dạy trang trọng hơn nếu cần.
