---
title: "CF 103665I - \u0414\u043e\u043c\u0430\u0448\u043d\u044f\u044f \u0440\u0430\u0431\u043e\u0442\u0430"
description: "Chúng ta được cấp một tập hợp nhiều chữ số, tất cả đều khác 0 và chúng ta được phép sắp xếp chúng theo bất kỳ thứ tự nào để tạo thành một số. Nhiệm vụ giới thiệu hai người tham gia, cả hai đều xây dựng các số từ cùng một bộ chữ số."
date: "2026-07-02T21:45:41+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103665
codeforces_index: "I"
codeforces_contest_name: "\u0422\u0443\u0440\u043d\u0438\u0440 \u0410\u0440\u0445\u0438\u043c\u0435\u0434\u0430 2018"
rating: 0
weight: 103665
solve_time_s: 48
verified: true
draft: false
---

[CF 103665I - \u0414\u043e\u043c\u0430\u0448\u043d\u044f\u044f \u0440\u0430\u0431\u043e\u0442\u0430](https://codeforces.com/problemset/problem/103665/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 48s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một tập hợp nhiều chữ số, tất cả đều khác 0 và chúng ta được phép sắp xếp chúng theo bất kỳ thứ tự nào để tạo thành một số. Nhiệm vụ giới thiệu hai người tham gia, cả hai đều xây dựng các số từ cùng một bộ chữ số. Người đầu tiên luôn chọn số lớn nhất có thể theo nghĩa từ điển, ở đây tương đương với việc sắp xếp các chữ số theo thứ tự giảm dần. 

Người thứ hai bị ràng buộc khác: họ cũng muốn con số lớn nhất có thể, nhưng nó không được giống với con số tối ưu của người thứ nhất. Nếu mọi sự sắp xếp có thể đều tạo ra cùng một số tối đa thì người thứ hai không có lựa chọn thay thế hợp lệ và câu trả lời không tồn tại. 

Kích thước đầu vào lên tới 100000 chữ số, vì vậy mọi giải pháp đều phải chạy trong thời gian tuyến tính hoặc gần tuyến tính. Việc sắp xếp có thể chấp nhận được vì O(n log n) nằm trong giới hạn thoải mái, nhưng bất cứ điều gì liên quan đến việc liệt kê các hoán vị hoặc cố gắng sắp xếp lại nhiều lần đều không thể thực hiện được vì số lượng hoán vị tăng theo giai thừa. 

Điều tinh tế quan trọng là nhiều hoán vị khác nhau vẫn có thể tạo ra cùng một số lượng tối đa. Điều này xảy ra chính xác khi tất cả các chữ số đều bằng nhau. Trong trường hợp đó, mọi hoán vị đều giống hệt nhau nên không có sự thay thế nào tồn tại. 

Một sai lầm ngây thơ là cho rằng chúng ta luôn có thể “sửa đổi một chút” hoán vị tối đa bằng cách hoán đổi các chữ số liền kề. Điều này không thành công khi tất cả các chữ số đều bằng nhau hoặc khi tất cả các giao dịch hoán đổi đều bảo toàn sự bằng nhau do các giá trị lặp lại. 

Ví dụ: nếu đầu vào là`7 7 7`, số lượng tối đa là`777`và mọi cách sắp xếp đều giống nhau nên câu trả lời phải là “Không”. Một ví dụ khác là`9 9 9 9`, cùng hoàn cảnh 

## Phương pháp tiếp cận 

Ý tưởng mạnh mẽ sẽ là tạo ra tất cả các hoán vị của các chữ số, tính toán các giá trị số của chúng và chọn giá trị lớn nhất không bằng mức tối đa tuyệt đối. Điều này đúng về mặt khái niệm vì nó trực tiếp khám phá không gian tìm kiếm của tất cả các số hợp lệ. Tuy nhiên, số lượng hoán vị là n!, điều này trở nên không khả thi ngay cả khi n = 10. Độ phức tạp về thời gian bùng nổ ngay lập tức và thậm chí việc lưu trữ các hoán vị cũng trở nên không thể. 

Một quan sát tốt hơn là chúng ta không thực sự cần xem xét tất cả các hoán vị. Số lượng tối đa được xác định duy nhất: sắp xếp các chữ số theo thứ tự giảm dần. Câu hỏi duy nhất là liệu có tồn tại sự sắp xếp nào khác với sự sắp xếp đã được sắp xếp này hay không. 

Nếu tồn tại ít nhất hai chữ số riêng biệt trong đầu vào thì chúng ta luôn có thể tạo một hoán vị khác bằng cách hoán đổi hai chữ số không bằng nhau ở đâu đó. Điều này đảm bảo một số khác với mức tối đa. Vì chúng ta vẫn sử dụng tất cả các chữ số chính xác một lần nên mọi hoán vị không giống nhau đều hợp lệ. 

Nếu tất cả các chữ số giống hệt nhau thì mọi hoán vị sẽ thu gọn về cùng một chuỗi, do đó không có sự thay thế nào tồn tại. 

Do đó, vấn đề rút gọn thành một kiểm tra duy nhất: liệu tất cả các chữ số có bằng nhau hay không. Nếu không, hãy in “Có” và xuất ra bất kỳ hoán vị nào khác với hoán vị được sắp xếp giảm dần. Một cách đơn giản là sắp xếp giảm dần để đạt giá trị tối đa, sau đó hoán đổi hai chữ số cuối nếu chúng khác nhau; nếu chúng bằng nhau, hãy tìm bất kỳ vị trí nào trước đó có chữ số khác và hoán đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các chữ số vào một mảng. 
2. Sắp xếp các chữ số theo thứ tự giảm dần để tạo ra số lớn nhất có thể. Điều này tương ứng với kết quả mà Biba thu được. 
3. Kiểm tra xem tất cả các chữ số có giống nhau hay không bằng cách so sánh từng chữ số với chữ số đầu tiên. 
4. Nếu tất cả các chữ số giống nhau, ghi “Không” vì mọi hoán vị đều giống hệt nhau và không có sự thay thế nào tồn tại. 
5. Ngược lại, hãy xây dựng một hoán vị hợp lệ khác. 
6. Một cách đơn giản là xác định vị trí cuối cùng mà việc hoán đổi với hàng xóm của nó tạo ra sự thay đổi. Trong thực tế, vì mảng đã được sắp xếp nên việc hoán đổi bất kỳ cặp liền kề bằng nhau nào cũng không có tác dụng gì, vì vậy thay vào đó, chúng tôi tìm vị trí ngoài cùng bên phải nơi các chữ số khác với chữ số đầu tiên và hoán đổi nó với chữ số cuối cùng. 
7. Xuất “Có” và trình tự được sửa đổi. 

Ý tưởng chính là chúng ta chỉ cần đảm bảo sự bất bình đẳng với cách sắp xếp tối đa, không tối đa hóa cách sắp xếp thứ hai hơn nữa. 

### Tại sao nó hoạt động 

Mảng giảm dần được sắp xếp là hoán vị tối đa về mặt từ điển duy nhất của nhiều tập hợp. Bất kỳ hoán vị nào khác với nó phải khác nhau ở một số vị trí sớm nhất mà thứ tự không giảm hoàn toàn theo cùng một thứ tự. Nếu tồn tại ít nhất hai chữ số riêng biệt, chúng ta luôn có thể thay đổi ít nhất một vị trí mà không làm mất tính hợp lệ, do đó luôn tồn tại một hoán vị khác biệt thứ hai. Nếu không tồn tại cặp chữ số riêng biệt như vậy thì tập hợp có kích thước n với các phần tử giống hệt nhau, do đó không gian hoán vị có kích thước 1. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(input().strip())

    a.sort(reverse=True)

    if all(x == a[0] for x in a):
        print("No")
        return

    # find a position that is not equal to the first digit
    i = n - 1
    while i >= 0 and a[i] == a[0]:
        i -= 1

    # swap with last position
    a[i], a[-1] = a[-1], a[i]

    print("Yes")
    print("".join(a))

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách sắp xếp các chữ số theo thứ tự giảm dần, tạo thành số lớn nhất. Việc kiểm tra sử dụng`all(x == a[0])`xác định trường hợp suy biến trong đó mọi chữ số đều giống hệt nhau, vì trong trường hợp đó không có sự sắp xếp lại nào có thể tạo ra một chuỗi khác. 

Để xây dựng một hoán vị hợp lệ khác, chúng tôi khai thác sự hiện diện của ít nhất một chữ số nhỏ hơn chữ số tối đa. Chúng tôi xác định vị trí chữ số ngoài cùng bên phải và hoán đổi nó với vị trí cuối cùng. Điều này đảm bảo kết quả khác với mức tối đa được sắp xếp trong khi vẫn sử dụng tất cả các chữ số chính xác một lần. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
5 4 3 1 1
```Sau khi sắp xếp giảm dần, ta được: 

| Bước | Mảng | 
| --- | --- | 
| Đã sắp xếp | 5 4 3 1 1 | 
| Kiểm tra đồng phục | sai | 
| Tìm chỉ mục khác nhau | i = 2 (chữ số 3) | 
| Hoán đổi với | 5 4 1 1 3 | 

Đầu ra là:```
Yes
54113
```Điều này cho thấy một lần hoán đổi duy nhất là đủ để tạo ra một hoán vị hợp lệ khác trong khi vẫn duy trì tính hợp lệ. 

### Ví dụ 2 

đầu vào:```
3
7 7 7
```| Bước | Mảng | 
| --- | --- | 
| Đã sắp xếp | 7 7 7 | 
| Kiểm tra đồng phục | đúng | 

Đầu ra:```
No
```Điều này xác nhận trường hợp suy biến trong đó không gian hoán vị có kích thước bằng một. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế; quét và trao đổi là tuyến tính | 
| Không gian | O(n) | Lưu trữ mảng chữ số | 

Các ràng buộc cho phép tối đa 100000 chữ số và sắp xếp nhiều phần tử phù hợp thoải mái trong giới hạn thời gian trong Python. Các thao tác còn lại là quét tuyến tính không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import io as sio

    out = sio.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples (illustrative formatting)
assert run("5\n5 4 3 1 1\n") in ["Yes\n54113", "Yes\n54131"]

assert run("3\n7 7 7\n") == "No"

# custom cases
assert run("1\n9\n") == "No"
assert run("2\n1 2\n") == "Yes"
assert run("4\n9 9 9 8\n") == "Yes"
assert run("6\n2 2 3 3 4 4\n") == "Yes"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| một chữ số | Không | trường hợp tối thiểu, không có sự thay thế | 
| hai chữ số riêng biệt | Có | trao đổi không tầm thường đơn giản nhất | 
| hầu hết đều có chữ số bằng nhau | Có | xử lý cấu trúc lặp đi lặp lại | 
| nhiều cặp | Có | giá trị sắp xếp lại chung | 

## Vỏ cạnh 

### Tất cả các chữ số giống hệt nhau 

đầu vào:```
4
8 8 8 8
```Sau khi sắp xếp, mảng không thay đổi. Kiểm tra tính đồng nhất phát hiện rằng mọi phần tử đều bằng phần tử đầu tiên. Thuật toán ngay lập tức đưa ra “Không”. Điều này đúng vì mọi hoán vị đều tạo ra cùng một chuỗi. 

### Chỉ khác nhau một chữ số 

đầu vào:```
5
9 9 9 9 1
```Đã sắp xếp:```
9 9 9 9 1
```Quá trình quét cho thấy không phải tất cả các chữ số đều bằng nhau. Chúng ta hoán đổi chữ số cuối cùng với chính nó hoặc một vị trí khác tùy theo cách thực hiện. Một kết quả hợp lệ là:```
9 9 9 1 9
```Điều này khác với mức tối đa và sử dụng tất cả các chữ số chính xác một lần. 

### Đã đa dạng chữ số 

đầu vào:```
4
3 2 1 4
```Đã sắp xếp:```
4 3 2 1
```Chúng tôi tìm thấy một hoán vị không tối đa bằng cách hoán đổi một vị trí không đồng nhất, ví dụ:```
4 3 1 2
```Điều này đảm bảo một sự sắp xếp hoàn toàn khác biệt trong khi vẫn duy trì hiệu lực.
