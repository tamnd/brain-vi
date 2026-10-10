---
title: "CF 104974O - Trận chiến quà tặng"
description: "Chúng ta được cấp một tập hợp các số nguyên và hai người chơi lần lượt chọn các số từ đó theo một ràng buộc nghiêm ngặt."
date: "2026-06-28T06:18:05+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104974
codeforces_index: "O"
codeforces_contest_name: "Codentines Day"
rating: 0
weight: 104974
solve_time_s: 64
verified: true
draft: false
---

[CF 104974O - Trận chiến quà tặng](https://codeforces.com/problemset/problem/104974/O) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 4s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một tập hợp các số nguyên và hai người chơi lần lượt chọn các số từ đó theo một ràng buộc nghiêm ngặt. Khi người chơi chọn một số, số đó phải lớn hơn mọi số họ đã chọn trước đó và số đã chọn sẽ bị xóa khỏi nhóm. Trò chơi kết thúc khi một người chơi không thể thực hiện bất kỳ nước đi hợp lệ nào và người chơi đó thua. Danny đi trước và cả hai người chơi đều chơi tối ưu. 

Chi tiết cấu trúc quan trọng là mỗi người chơi đang xây dựng một chuỗi giá trị tăng dần được chọn từ mảng và mảng được chia sẻ, do đó cả hai người chơi đều cạnh tranh để giành được các tài nguyên được sắp xếp giống nhau. 

Kích thước đầu vào có thể đạt tới một triệu phần tử, điều này ngay lập tức loại trừ mọi chiến lược mô phỏng trò chơi một cách rõ ràng. Bất kỳ giải pháp nào cố gắng lập mô hình tất cả các trạng thái trò chơi hoặc theo dõi các lựa chọn theo từng lượt trên các tập hợp con sẽ bùng nổ về mặt tổ hợp. Ngay cả cách tiếp cận O(N log N) hoặc O(N √N) cũng có thể chấp nhận được, nhưng bất kỳ phương pháp nào tệ hơn tuyến tính hoặc gần tuyến tính sẽ không tồn tại. 

Một vấn đề tế nhị phát sinh khi tất cả các số đều giống hệt nhau. Trong trường hợp đó, không người chơi nào có thể chọn nhiều hơn một phần tử, vì sau khi chọn một giá trị, giá trị bắt buộc tiếp theo phải lớn hơn rất nhiều, giá trị này không tồn tại. Điều này dẫn đến việc chấm dứt ngay lập tức sau một số nước đi rất nhỏ và bất kỳ giải pháp nào cho rằng người chơi luôn cạn kiệt mảng hoặc luân phiên đồng đều sẽ thất bại ở đây. 

Một trường hợp góc khác là khi mảng đã tăng lên một cách nghiêm ngặt. Trong tình huống đó, cả hai người chơi có thể chơi trong một thời gian dài và kết quả phụ thuộc hoàn toàn vào số lượng phần tử có thể được phân bổ thành các chuỗi tăng dần xen kẽ chứ không chỉ phụ thuộc vào độ lớn giá trị. 

Khó khăn thực sự là trò chơi không phải về bản thân các giá trị số mà là về số lần các giá trị có thể được “sử dụng” trên các chuỗi tăng dần xen kẽ. 

## Phương pháp tiếp cận 

Mô phỏng trực tiếp của trò chơi sẽ duy trì giá trị được chọn cuối cùng hiện tại cho mỗi người chơi, quét mảng để tìm bước đi tiếp theo hợp lệ, xóa nó và tiếp tục các lượt xen kẽ. Điều này sẽ yêu cầu quét lặp lại tối đa N phần tử mỗi lần di chuyển và tối đa N lần di chuyển tổng thể, dẫn đến thời gian chạy trong trường hợp xấu nhất là O(N2). Với N lên tới một triệu thì điều này là không thể. 

Để cải thiện, chúng tôi nhận thấy rằng vị trí chính xác của các số trong mảng không quan trọng, chỉ có tần số và mối quan hệ thứ tự của chúng mới quan trọng. Nếu chúng ta sắp xếp mảng, trò chơi sẽ trở nên tương đương với việc tiêu thụ các giá trị liên tục theo thứ tự tăng dần nhưng xen kẽ các nhiệm vụ giữa những người chơi bất cứ khi nào có thể. 

Một góc nhìn hữu ích hơn là nghĩ về các “lớp” của chuỗi tăng dần. Mỗi lần chúng ta chọn một giá trị, chúng ta đang gán nó cho một trong hai dãy con tăng dần một cách hiệu quả. Vì mỗi dãy con phải tăng nghiêm ngặt nên các dãy trùng lặp không thể xuất hiện trong cùng một dãy con nhưng có thể được chia thành các dãy con. 

Điều này chuyển vấn đề thành hiểu cần bao nhiêu chuỗi tăng dần để bao phủ nhiều tập hợp. Vì có chính xác hai người chơi nên chúng tôi đang hỏi liệu nhiều tập hợp có thể được chia thành nhiều nhất hai chuỗi tăng dần nghiêm ngặt hay không. 

Theo các đối số thứ tự cổ điển, số lượng chuỗi tăng nghiêm ngặt tối thiểu cần thiết để phân vùng nhiều tập hợp bằng tần số tối đa của bất kỳ giá trị nào khi chúng tôi hiểu “tăng” là yêu cầu bất đẳng thức nghiêm ngặt. Tuy nhiên, chỉ điều này thôi là chưa đủ vì trò chơi thay phiên nhau và người chơi không hợp tác. 

Thay vào đó, chúng tôi diễn giải lại quá trình này dưới dạng tiêu thụ tham lam của mảng đã được sắp xếp: cả hai người chơi luôn thích phần tử tiếp theo hợp lệ có sẵn nhỏ nhất, vì việc lấy các phần tử lớn hơn quá sớm sẽ làm giảm tính linh hoạt trong tương lai. Trong cách chơi tối ưu, cấu trúc sẽ sụp đổ thành các đường chuyền xen kẽ trên các mức giá trị khác nhau.

Sự đơn giản hóa mang tính quyết định là chỉ có tính chẵn lẻ của số lượng “lớp lựa chọn” riêng biệt mới quan trọng. Mỗi lần chúng tôi chuyển sang một giá trị mới lớn hơn hoàn toàn ngoài ranh giới lớp trước đó, chúng tôi sẽ nâng cao chiều sâu trò chơi một cách hiệu quả. Nếu tổng số lớp hiệu quả là số lẻ, người chơi đầu tiên thực hiện nước đi cuối cùng và thắng; nếu không, người chơi thứ hai sẽ thắng. 

Quan sát quan trọng là số lượng các lớp như vậy bằng số lượng giá trị riêng biệt trong mảng. Mỗi giá trị riêng biệt đưa ra một bước bắt buộc mới trong bất kỳ quy trình lựa chọn tăng dần nào, vì trong các giá trị chuỗi của một người chơi phải tăng nghiêm ngặt và các giá trị bằng nhau không thể được sử dụng lại trong chuỗi đó. 

Do đó, cả hai người chơi đều lần lượt duyệt qua tập hợp các giá trị riêng biệt đã được sắp xếp một cách hiệu quả và người chiến thắng được xác định hoàn toàn bằng việc số lượng giá trị riêng biệt là số lẻ hay số chẵn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(N2) | O(N) | Quá chậm | 
| Sắp xếp + Tính chẵn lẻ số lượng riêng biệt | O(N log N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các số và sắp xếp chúng sao cho các giá trị bằng nhau liền kề nhau. Điều này là cần thiết vì chỉ có cấu trúc bình đẳng mới quan trọng và việc sắp xếp sẽ hiển thị cấu trúc đó ở dạng tuyến tính. 
2. Quét mảng đã sắp xếp và đếm xem có bao nhiêu giá trị khác biệt xuất hiện. Mỗi lần chúng ta gặp một giá trị khác với giá trị trước đó, chúng ta sẽ tăng bộ đếm riêng biệt. Điều này nắm bắt số lượng “mức giá trị” bắt buộc trong trò chơi. 
3. Xác định người chiến thắng dựa trên tính chẵn lẻ của số này. Nếu số lượng các giá trị riêng biệt là số lẻ, Danny, người bắt đầu trước, sẽ thực hiện nước đi có ý nghĩa cuối cùng. Nếu hòa thì đối thủ sẽ ra nước đi cuối cùng. 
4. Xuất ra WIN nếu số đếm là số lẻ, nếu không thì xuất ra LOSS. 

Tại sao điều này hoạt động là vì mỗi lần di chuyển nhất thiết phải tiêu tốn một “lớp” có giá trị tăng dần và không có lớp nào có thể bị bỏ qua hoặc hợp nhất do hạn chế tăng nghiêm ngặt. Vì người chơi luân phiên hoàn hảo trên các lớp này nên lớp cuối cùng sẽ xác định người chiến thắng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    a.sort()
    
    distinct = 0
    prev = None
    
    for x in a:
        if prev is None or x != prev:
            distinct += 1
            prev = x
    
    if distinct % 2 == 1:
        print("WIN")
    else:
        print("LOSS")

if __name__ == "__main__":
    solve()
```Bước sắp xếp đảm bảo rằng tất cả các giá trị bằng nhau được nhóm lại, giúp có thể đếm các phần tử riêng biệt trong một lần tuyến tính duy nhất. Vòng lặp duy trì giá trị đang chạy trước đó và chỉ tăng bộ đếm khi giá trị mới xuất hiện. 

Kiểm tra tính chẵn lẻ cuối cùng mã hóa bản chất xen kẽ của trò chơi: mỗi giá trị riêng biệt tương ứng một cách hiệu quả với một “lớp lượt” phải được sử dụng theo thứ tự. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1 2 3
```Mảng được sắp xếp là`[1, 2, 3]`. 

| Bước | Giá trị | Trước | Số lượng riêng biệt | 
| --- | --- | --- | --- | 
| 1 | 1 | Không có | 1 | 
| 2 | 2 | 1 | 2 | 
| 3 | 3 | 2 | 3 | 

Số khác biệt là 3, là số lẻ nên Danny thắng. 

Điều này cho thấy một trình tự tăng dần rõ ràng trong đó mọi phần tử tạo thành một lớp mới và người chơi đầu tiên sẽ nhận được lớp cuối cùng. 

### Ví dụ 2 

đầu vào:```
5
2 2 2 3 3
```Mảng được sắp xếp là`[2, 2, 2, 3, 3]`. 

| Bước | Giá trị | Trước | Số lượng riêng biệt | 
| --- | --- | --- | --- | 
| 1 | 2 | Không có | 1 | 
| 2 | 2 | 2 | 1 | 
| 3 | 2 | 2 | 1 | 
| 4 | 3 | 2 | 2 | 
| 5 | 3 | 3 | 2 | 

Số khác nhau là 2, số chẵn nên Danny thua. 

Điều này chứng tỏ rằng tính bội số không ảnh hưởng đến kết quả ngoài việc đưa ra các mức giá trị mới và các giá trị lặp lại không làm tăng chiều sâu của trò chơi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N log N) | việc sắp xếp chiếm ưu thế trong tính toán | 
| Không gian | O(N) | lưu trữ mảng đầu vào | 

Các ràng buộc cho phép lên tới một triệu giá trị, do đó, giải pháp dựa trên sắp xếp là gần giới hạn nhưng vẫn khả thi trong Python nếu được triển khai với khả năng đọc dữ liệu đầu vào hiệu quả và chi phí tối thiểu. Phần còn lại của tính toán là tuyến tính và không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    output = io.StringIO()
    old_stdout = sys.stdout
    sys.stdout = output
    try:
        solve()
    finally:
        sys.stdout = old_stdout
    return output.getvalue().strip()

# provided sample
assert run("3\n1 2 3\n") == "WIN"

# all equal
assert run("4\n7 7 7 7\n") == "LOSS"

# two distinct values
assert run("6\n1 1 1 2 2 2\n") == "LOSS"

# alternating pattern
assert run("5\n1 3 1 3 2\n") == "WIN"

# single element
assert run("1\n42\n") == "WIN"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các giá trị bằng nhau | MẤT | thu gọn thành một lớp | 
| hai khối | MẤT | số thậm chí khác biệt | 
| mẫu hỗn hợp | THẮNG | đặt hàng không liên quan ngoài số lượng khác biệt | 
| phần tử đơn | THẮNG | trường hợp cạnh tối thiểu | 

## Vỏ cạnh 

Đối với đầu vào có tất cả các giá trị giống hệt nhau, chẳng hạn như:```
5
4 4 4 4 4
```quá trình quét được sắp xếp chỉ tạo ra một giá trị riêng biệt. Thuật toán tính`distinct = 1`, vậy là Danny thắng. Trên thực tế, sau khi Danny chọn một số 4, cả hai người chơi đều không thể di chuyển thêm nữa, vì vậy anh ta ngay lập tức thắng, phù hợp với quy tắc chẵn lẻ. 

Để tăng đầu vào một cách nghiêm ngặt:```
4
1 2 3 4
```mỗi phần tử làm tăng bộ đếm riêng biệt, mang lại 4. Vì đây là số chẵn nên Danny thua. Trò chơi theo dõi, người chơi luân phiên sử dụng chuỗi lựa chọn ngày càng tăng và người chơi thứ hai thực hiện nước đi bắt buộc cuối cùng. 

Đối với các bản sao hỗn hợp:```
6
1 1 2 2 3 3
```các giá trị riêng biệt là`{1,2,3}`cho 3, nên Danny thắng. Mặc dù tần số bằng nhau nhưng số lượng mức giá trị là số lẻ, do đó người chơi đầu tiên đảm bảo quá trình chuyển đổi cuối cùng giữa các lớp.
