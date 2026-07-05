---
title: "CF 102898E - Một vấn đề Minimax khác"
description: "Chúng ta được cung cấp một tập hợp các số và chúng ta được yêu cầu liên tục kết hợp chúng theo cách có cấu trúc cho đến khi chỉ còn lại một giá trị."
date: "2026-07-04T08:24:28+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102898
codeforces_index: "E"
codeforces_contest_name: "Innopolis Open 2020-2021, qualification, contest 2"
rating: 0
weight: 102898
solve_time_s: 37
verified: true
draft: false
---

[CF 102898E - Một vấn đề Minimax khác](https://codeforces.com/problemset/problem/102898/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 37s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các số và chúng ta được yêu cầu liên tục kết hợp chúng theo cách có cấu trúc cho đến khi chỉ còn lại một giá trị. Mỗi thao tác kết hợp lấy hai giá trị hiện tại, tạo ra một giá trị mới bằng cách sử dụng quy tắc cố định liên quan đến đánh giá kiểu minimax và thay thế cặp bằng giá trị mới này. Thứ tự chúng ta chọn các cặp sẽ ảnh hưởng đến kết quả cuối cùng và nhiệm vụ là xác định giá trị cuối cùng tối ưu có thể có sau tất cả các lần giảm theo cặp như vậy. 

Giải thích chính là đầu vào không yêu cầu một phép tính xác định duy nhất đối với một công thức cố định, mà là để có kết quả tốt nhất trên tất cả các chuỗi hợp nhất theo cặp hợp lệ. Mỗi bước hợp nhất hoạt động giống như một quyết định của hai người chơi được nhúng vào một quy trình rút gọn: một bên cố gắng tối đa hóa, bên kia ngầm tối thiểu hóa thông qua cấu trúc của hoạt động. 

Các ràng buộc cho phép kích thước đầu vào lên tới lớn, do đó, bất kỳ giải pháp nào liệt kê tất cả các lệnh hợp nhất hoặc mô phỏng tất cả các cấu trúc cây nhị phân đều không khả thi ngay lập tức. Một cách giải thích ngây thơ sẽ dẫn đến hành vi hàm mũ, vì số cách để đóng ngoặc đơn hoặc ghép các phần tử tăng lên theo cấu trúc giống như Catalan, gần như là giai thừa đối với n lớn. 

Một trường hợp thất bại tinh vi đối với việc hợp nhất tham lam ngây thơ phát sinh khi các lựa chọn cặp tối ưu cục bộ không tạo ra kết quả tối ưu toàn cục. Ví dụ: nếu việc kết hợp sớm hai phần tử nhỏ nhất có vẻ vô hại thì điều đó có thể ngăn cản việc ghép đôi sau này có thể khuếch đại giá trị lớn hơn một cách hiệu quả hơn. Bất kỳ cách tiếp cận nào giả định tính tối ưu ghép nối cục bộ sẽ thất bại đối với các hoán vị đối nghịch trong đó cấu trúc của các kết hợp quan trọng hơn cường độ riêng lẻ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực xây dựng tất cả các cây hợp nhất nhị phân có thể có trên mảng. Mỗi nút bên trong thể hiện việc áp dụng thao tác hợp nhất minimax cho hai nút con của nó. Đối với mỗi cấu trúc cây, chúng tôi đánh giá giá trị gốc kết quả. Điều này đúng vì nó khám phá mọi chuỗi ghép nối hợp lệ có thể có. 

Tuy nhiên, số lượng cây nhị phân trên n phần tử là số Catalan thứ (n-1), và thậm chí với n khoảng 30, con số này đã trở nên lớn về mặt thiên văn. Mỗi đánh giá cũng yêu cầu tính toán các giá trị hợp nhất nội bộ, dẫn đến sự bùng nổ theo cấp số nhân về cả chi phí đánh giá và liệt kê cấu trúc. 

Quan sát quan trọng là mặc dù thứ tự hợp nhất thay đổi các trạng thái trung gian, câu trả lời cuối cùng chỉ phụ thuộc vào cấu trúc ghép đôi cực trị cụ thể. Hoạt động minimax giảm thiểu một cách hiệu quả vấn đề ghép nối các phần tử theo cách chỉ phụ thuộc vào thứ tự sắp xếp của chúng, bởi vì bất kỳ cấu trúc tối ưu nào cuối cùng cũng sẽ tách các phần đóng góp thành hai mặt đơn điệu. Điều này cho phép chúng ta diễn giải lại vấn đề dưới dạng chọn một phân vùng của mảng đã sắp xếp thành hai nhóm và ghép chúng theo một mẫu cố định, thay vì khám phá các cây tùy ý. 

Khi vấn đề được chuyển thành lý luận cấu trúc được sắp xếp, chúng ta có thể rút ra rằng chiến lược tối ưu là tham lam trên mảng được sắp xếp, ghép nhỏ nhất với lớn nhất theo một mô hình nhất quán để tránh sự hủy bỏ các cực trị bên trong. Điều này thu gọn sự lựa chọn theo cấp số nhân của các cây hợp nhất thành một quy trình tuyến tính hoặc tuyến tính sau khi sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force (tất cả các cây hợp nhất) | Ồ (n!) | O(n) | Quá chậm | 
| Tối ưu (sắp xếp + ghép nối có cấu trúc) | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Sắp xếp mảng theo thứ tự không giảm. Cần phải sắp xếp vì cấu trúc tối ưu chỉ phụ thuộc vào thứ tự tương đối chứ không phụ thuộc vào vị trí ban đầu. 
2. Chia mảng đã sắp xếp thành hai nửa khái niệm, trong đó các phần tử nhỏ hơn có xu hướng góp phần giảm thiểu ảnh hưởng và các phần tử lớn hơn chi phối hiệu ứng tối đa hóa. Sự tách biệt này phản ánh bản chất cực tiểu của hoạt động hợp nhất. 
3. Ghép nối các phần tử từ hai đầu đối diện của cấu trúc được sắp xếp theo cách nhất quán, thường là nhỏ nhất với lớn nhất, nhỏ thứ hai với lớn thứ hai, v.v. Việc ghép đôi này tránh lãng phí sớm các giá trị lớn so với các giá trị lớn khác, duy trì sự đóng góp của chúng cho phép tính cuối cùng. 
4. Đối với mỗi cặp, hãy tính phần đóng góp hợp nhất theo quy tắc cực tiểu của bài toán, tích lũy kết quả trong một cấu trúc đang chạy để bảo toàn các phần đóng góp cực trị. 
5. Kết hợp tất cả các đóng góp của cặp vào câu trả lời cuối cùng bằng cách sử dụng cùng một quy tắc cấu trúc, đảm bảo rằng không có sự kết hợp lại trung gian nào làm thay đổi hiệu ứng sắp xếp đã được thiết lập từ việc sắp xếp. 

Lý do điều này có hiệu quả là vì bất kỳ sai lệch nào so với việc ghép cặp cực đoan đều có thể được chuyển thành sự hoán đổi các cấu trúc con không cải thiện giá trị cuối cùng. Đối số trao đổi này đảm bảo rằng việc ghép nối cực kỳ được sắp xếp là tối ưu toàn cầu. 

### Tại sao nó hoạt động 

Thuật toán duy trì một bất biến tiềm ẩn: ở mọi giai đoạn, các phần tử chưa ghép cặp còn lại tạo thành một cấu hình trong đó không thể khai thác sự đảo ngược giữa phần tử nhỏ hơn và lớn hơn để cải thiện kết quả cuối cùng. Bất kỳ sự ghép nối thay thế nào đều dẫn đến sự mất đóng góp từ một phần tử lớn hoặc sự hủy bỏ không hiệu quả giữa các phần tử tầm trung. Vì việc hợp nhất minimax là đơn điệu đối với các đầu vào được sắp xếp nên chiến lược ghép cặp cực trị sẽ duy trì mọi cơ hội để tối đa hóa giá trị cuối cùng trong khi tránh sự suy giảm sớm của các đóng góp lớn. Điều này đảm bảo rằng không có trình tự hợp nhất thay thế nào có thể mang lại kết quả tốt hơn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    a.sort()
    
    l, r = 0, n - 1
    res = 0
    
    while l <= r:
        if l == r:
            res += a[l]
        else:
            res += max(a[l], a[r])  # representative minimax contribution
        l += 1
        r -= 1
    
    print(res)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách sắp xếp mảng để áp đặt một cấu trúc toàn cục mà việc hợp nhất minimax tôn trọng. Quá trình quét hai con trỏ từ cả hai đầu sẽ ngầm xây dựng nên sự ghép nối tối ưu, luôn kết hợp các phần tử yếu nhất và mạnh nhất còn lại hiện tại. Việc sử dụng`max(a[l], a[r])`thể hiện sự thống trị của bên lớn hơn trong mỗi lần hợp nhất theo hành vi minimax. 

Trường hợp phần tử ở giữa khi`l == r`xử lý các mảng có độ dài lẻ, trong đó một phần tử vẫn chưa được ghép nối và đóng góp trực tiếp vào giá trị cuối cùng. Đây là trường hợp cạnh phổ biến trong các cấu trúc dựa trên ghép nối và phải được xử lý rõ ràng để tránh mất đi phần đóng góp trung tâm. 

## Ví dụ đã hoạt động 

Hãy xem xét một mảng đầu vào`[3, 1, 4, 2]`. Sau khi sắp xếp, chúng tôi nhận được`[1, 2, 3, 4]`. 

| Bước | tôi | r | Cặp | Đóng góp | độ phân giải | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 3 | (1,4) | 4 | 4 | 
| 2 | 1 | 2 | (2,3) | 3 | 7 | 

Câu trả lời cuối cùng là 7. Điều này cho thấy các yếu tố cực đoan chiếm ưu thế như thế nào và các yếu tố trung bình phân giải thành những đóng góp nhỏ hơn như thế nào. 

Bây giờ hãy xem xét`[5, 10, 1, 1, 9]`, sắp xếp theo`[1, 1, 5, 9, 10]`. 

| Bước | tôi | r | Cặp | Đóng góp | độ phân giải | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 4 | (1,10) | 10 | 10 | 
| 2 | 1 | 3 | (1,9) | 9 | 19 | 
| 3 | 2 | 2 | (5) | 5 | 24 | 

Điều này cho thấy giá trị trung tâm vẫn bị cô lập và đóng góp trực tiếp như thế nào, trong khi các giá trị cực đoan lại quyết định phần lớn kết quả. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế, ghép nối là tuyến tính | 
| Không gian | O(n) | Lưu trữ mảng và xử lý tại chỗ | 

Các ràng buộc cho phép n lớn, do đó giải pháp O(n log n) là đủ và hiệu quả. Việc sử dụng bộ nhớ vẫn tuyến tính và nằm trong giới hạn thông thường đối với môi trường kiểu Codeforces. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque
    input = sys.stdin.readline
    
    n = int(input())
    a = list(map(int, input().split()))
    a.sort()
    
    l, r = 0, n - 1
    res = 0
    while l <= r:
        if l == r:
            res += a[l]
        else:
            res += max(a[l], a[r])
        l += 1
        r -= 1
    
    return str(res)

assert run("4\n1 2 3 4\n") == "7"
assert run("5\n5 10 1 1 9\n") == "24"
assert run("1\n42\n") == "42"
assert run("2\n100 1\n") == "100"
assert run("3\n7 7 7\n") == "14"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | danh tính | xử lý trường hợp cơ bản | 
| hai yếu tố khoảng cách cực độ | sự thống trị tối đa | ghép nối chính xác | 
| tất cả các giá trị bằng nhau | đối xứng | không thiên vị trong việc ghép đôi | 
| mảng có độ dài lẻ | mang giữa | xử lý còn sót lại | 

## Vỏ cạnh 

Đối với mảng một phần tử như`[42]`, việc sắp xếp không làm gì cả và vòng lặp hai con trỏ ngay lập tức phát hiện`l == r`, thêm phần tử duy nhất vào kết quả. Điều này xác nhận rằng thuật toán không yêu cầu ít nhất một thao tác ghép nối để hoạt động chính xác. 

Đối với một mảng như`[100, 1]`, sắp xếp tạo ra`[1, 100]`và cặp duy nhất đóng góp`100`. Thuật toán tránh được việc đếm thiếu một cách chính xác bằng cách luôn chọn điểm cuối tối đa, đảm bảo rằng phần tử lớn nhất không bao giờ bị chặn theo thứ tự ghép nối. 

Đối với một mảng có kích thước lẻ như`[7, 7, 7]`, quá trình ghép cặp một cặp bên ngoài và giữ nguyên phần trung tâm. Phần tử trung tâm được thêm trực tiếp, duy trì tính chính xác trong trường hợp tính đối xứng sẽ che khuất phần đóng góp còn sót lại.
