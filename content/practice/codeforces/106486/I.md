---
title: "CF 106486I - \u5976\u9f99\u5854"
description: "Chúng ta có một tập hợp các khối, mỗi khối có kích thước nguyên dương. Chúng tôi muốn xếp tất cả chúng vào một tháp thẳng đứng duy nhất. Quy tắc duy nhất là một khối chỉ có thể được đặt lên trên một khối khác nếu kích thước của nó nhỏ hơn khối bên dưới nó."
date: "2026-06-19T17:31:57+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106486
codeforces_index: "I"
codeforces_contest_name: "Dalian University of Technology, Software College 2025 Freshman Contest"
rating: 0
weight: 106486
solve_time_s: 48
verified: true
draft: false
---

[CF 106486I - \u5976\u9f99\u5854](https://codeforces.com/problemset/problem/106486/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 48s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một tập hợp các khối, mỗi khối có kích thước nguyên dương. Chúng tôi muốn xếp tất cả chúng vào một tháp thẳng đứng duy nhất. Quy tắc duy nhất là một khối chỉ có thể được đặt lên trên một khối khác nếu kích thước của nó nhỏ hơn khối bên dưới nó. Mỗi khối phải được sử dụng chính xác một lần và tất cả các khối phải thuộc cùng một tháp. 

Nhiệm vụ là quyết định xem có thể hình thành một tòa tháp như vậy hay không. 

Kích thước đầu vào có thể lớn tới 200000 khối và mỗi kích thước có thể lớn tới 10^9. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào cố gắng mô phỏng tất cả các thứ tự xếp chồng có thể có hoặc quay lại các hoán vị. Bất cứ điều gì bậc hai hoặc hàm mũ đều nằm ngoài tầm với. Một giải pháp về cơ bản phải là tuyến tính hoặc tuyến tính, rất có thể là O(n log n). 

Một điểm tế nhị là sự bình đẳng bị cấm. Nếu hai khối có cùng kích thước thì chúng không thể xếp chồng lên nhau theo một trong hai hướng. Điều này tạo ra một ràng buộc cứng: các giá trị bằng nhau hoạt động giống như các “độ cao” giống hệt nhau chặn lẫn nhau. 

Một trường hợp thường phá vỡ lý luận ngây thơ là khi tất cả các giá trị đều bằng nhau. Ví dụ, đầu vào`1 1`rõ ràng là không thể tạo thành một tòa tháp, bởi vì không cái nào có thể chồng lên cái kia. Một trường hợp cạnh quan trọng khác là mảng phần tử đơn như`5`, luôn luôn hợp lệ. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực sẽ là cố gắng xây dựng một thứ tự hợp lệ bằng cách chọn bất kỳ khối nào ở dưới cùng, sau đó liên tục chọn bất kỳ khối nhỏ hơn chưa sử dụng nào để đặt lên trên. Về cơ bản, điều này khám phá các hoán vị bằng cách kiểm tra ràng buộc ở mỗi bước. 

Điều này hoạt động về mặt khái niệm vì nó thực thi trực tiếp điều kiện giảm nghiêm ngặt. Tuy nhiên, ở mỗi lớp có thể có nhiều lựa chọn ứng viên và không gian tìm kiếm có kích thước theo giai thừa. Với n lên tới 200000 thì điều này hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là vấn đề không thực sự phụ thuộc vào các lựa chọn kề ngoài việc sắp xếp. Vì mọi khối phải xuất hiện theo một chuỗi giảm dần nghiêm ngặt, nên chúng ta đang đặt ra một câu hỏi đơn giản hơn: liệu chúng ta có thể sắp xếp tất cả các phần tử thành một chuỗi giảm dần nghiêm ngặt bằng cách sử dụng tất cả các phần tử chính xác một lần không? 

Điều đó chỉ có thể thực hiện được khi và chỉ khi tất cả các giá trị đều khác biệt. Nếu hai phần tử bằng nhau thì không có cách nào ra lệnh giảm chặt chúng mà không vi phạm quy tắc, vì đẳng thức phá vỡ tính chặt chẽ và chúng ta không thể “tách” chúng bằng các giá trị trung gian có cùng kích thước. 

Vì vậy, toàn bộ vấn đề giảm xuống còn việc kiểm tra xem có tồn tại bản sao hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Sắp xếp/Băm kiểm tra trùng lặp | O(n log n) hoặc O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các số đại diện cho kích thước khối. 
2. Phát hiện xem có giá trị nào xuất hiện nhiều lần hay không. 
3. Nếu tồn tại một bản sao, hãy kết luận ngay rằng không thể sắp xếp thứ tự nghiêm ngặt. 
4. Ngược lại, kết luận rằng tất cả các phần tử có thể được sắp xếp theo thứ tự giảm dần, do đó tồn tại một tháp hợp lệ. 

Lý do đằng sau bước 3 là bất kỳ cặp trùng lặp nào cũng sẽ vi phạm sự bất bình đẳng nghiêm ngặt ở đâu đó trong ngăn xếp cuối cùng, bất kể thứ tự. 

## Tại sao nó hoạt động 

Một tháp hợp lệ tương ứng chính xác với hoán vị của tất cả các phần tử được sắp xếp theo thứ tự giảm dần. Chuỗi giảm nghiêm ngặt yêu cầu mọi cặp liền kề phải thỏa mãn`a[i] > a[i+1]`. Nếu hai giá trị bằng nhau, chúng phải xuất hiện ở đâu đó trong chuỗi, nhưng bất kể chúng được đặt ở đâu, chúng sẽ trở nên liền kề hoặc bị phân tách bởi các giá trị nhỏ hơn. Trong cả hai trường hợp, một trong số chúng sẽ không duy trì được trật tự nghiêm ngặt so với các hàng xóm của nó trong một hoán vị đầy đủ. Do đó, sự tồn tại của bất kỳ bản sao nào khiến nhiệm vụ không thể thực hiện được và việc không có bản sao đảm bảo rằng việc sắp xếp theo thứ tự giảm dần sẽ tạo ra một tòa tháp hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input().strip())
a = list(map(int, input().split()))

seen = set()
for x in a:
    if x in seen:
        print("No")
        sys.exit(0)
    seen.add(x)

print("Yes")
```Việc triển khai trực tiếp theo dõi xem chúng ta đã thấy một giá trị nào trước đó hay chưa. Khi tìm thấy sự lặp lại, chúng tôi có thể chấm dứt sớm vì tháp hợp lệ là không thể. Nếu vòng lặp kết thúc, tất cả các giá trị đều khác biệt và tồn tại thứ tự giảm dần nghiêm ngặt bằng cách sắp xếp, mặc dù chúng tôi không xây dựng nó một cách rõ ràng. 

Lựa chọn thiết kế quan trọng là thoát sớm, giúp tránh những công việc không cần thiết trong trường hợp xấu nhất. Việc sử dụng một bộ đảm bảo kiểm tra thành viên O(1) trung bình, giữ cho giải pháp tuyến tính như kỳ vọng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`1 2 3 4 5`| Giá trị đã xử lý | Đã xem trước | Hành động | Tìm thấy trùng lặp | 
| --- | --- | --- | --- | 
| 1 | {} | thêm 1 | Không | 
| 2 | {1} | thêm 2 | Không | 
| 3 | {1,2} | thêm 3 | Không | 
| 4 | {1,2,3} | thêm 4 | Không | 
| 5 | {1,2,3,4} | thêm 5 | Không | 

Kết quả: Có 

Điều này xác nhận rằng khi tất cả các phần tử đều khác biệt thì sẽ tồn tại một trật tự chặt chẽ. 

### Ví dụ 2 

đầu vào:`1 1`| Giá trị đã xử lý | Đã xem trước | Hành động | Tìm thấy trùng lặp | 
| --- | --- | --- | --- | 
| 1 | {} | thêm 1 | Không | 
| 1 | {1} | dừng lại | Có | 

Kết quả: Không 

Điều này cho thấy trường hợp thất bại: đẳng thức ngay lập tức phá vỡ việc xếp chồng nghiêm ngặt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) trung bình | Mỗi phần tử được chèn và kiểm tra trong bộ băm một lần | 
| Không gian | O(n) | Lưu trữ các phần tử nhìn thấy trong trường hợp xấu nhất | 

Các ràng buộc cho phép tối đa 200000 phần tử, do đó, một giải pháp tuyến tính dễ dàng phù hợp trong giới hạn thời gian. Việc sử dụng bộ nhớ cũng nhỏ so với giới hạn 1024 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input().strip())
    a = list(map(int, input().split()))

    seen = set()
    for x in a:
        if x in seen:
            return "No"
        seen.add(x)
    return "Yes"

# provided samples
assert run("5\n1 2 3 4 5\n") == "Yes"
assert run("2\n1 1\n") == "No"

# custom cases
assert run("1\n10\n") == "Yes"
assert run("3\n3 2 1\n") == "Yes"
assert run("4\n1 2 2 3\n") == "No"
assert run("6\n6 5 4 3 2 1\n") == "Yes"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử | Có | trường hợp tối thiểu | 
| tất cả giảm dần rõ rệt | Có | cấu trúc đã hợp lệ | 
| một bản sao ở giữa | Không | phát hiện sự trùng lặp không liền kề | 
| thứ tự đảo ngược hoàn toàn | Có | trường hợp nghiêm ngặt tối đa | 

## Vỏ cạnh 

Một trường hợp phần tử duy nhất như`7`chuyển ngay lập tức vì không cần so sánh và tập hợp vẫn trống cho đến khi quá trình chèn hoàn tất. Thuật toán xuất ra chính xác Có. 

Một mảng hoàn toàn bằng nhau như`5 5 5 5`không thành công ở lần chèn thứ hai vì phần tử thứ hai đã có trong tập hợp. Việc phát hiện là ngay lập tức, do đó không cần xử lý thêm nữa. 

Một trường hợp hỗn hợp như`2 1 2`cũng thất bại một cách chính xác. Sau khi chèn`2`Và`1`, trận chung kết`2`kích hoạt kiểm tra trùng lặp ngay cả khi bản sao không liền kề, xác nhận rằng tính liền kề là không liên quan và chỉ có tính duy nhất toàn cầu mới quan trọng.
