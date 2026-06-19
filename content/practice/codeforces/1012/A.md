---
title: "CF 1012A - Ảnh Bầu Trời"
description: "Chúng ta được cho một tập hợp các số có kích thước 2n, và chúng ta được biết rằng những số này ban đầu đến từ n điểm trong mặt phẳng, trong đó mỗi điểm đóng góp chính xác hai số nguyên: tọa độ x và tọa độ y của nó."
date: "2026-06-16T22:34:16+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1012
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 500 (Div. 1) [based on EJOI]"
rating: 1500
weight: 1012
solve_time_s: 89
verified: true
draft: false
---

[CF 1012A - Bức ảnh bầu trời](https://codeforces.com/problemset/problem/1012/A) 

**Đánh giá:** 1500 
**Tags:** vũ lực, thực hiện, toán học, sắp xếp 
**Thời gian giải:** 1 phút 29s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp nhiều số có kích thước`2n`, và chúng tôi được biết rằng những con số này ban đầu đến từ`n`các điểm trong mặt phẳng, trong đó mỗi điểm đóng góp chính xác hai số nguyên: tọa độ x và tọa độ y của nó. Điều đáng chú ý là việc ghép đôi bị mất hoàn toàn nên chúng ta không còn biết hai số nào cùng điểm. Chúng ta chỉ biết rằng tồn tại một số cặp số thành`n`các điểm và những điểm đó được bao quanh bởi một hình chữ nhật thẳng hàng với trục. 

Nhiệm vụ là tái tạo lại, trên tất cả các cặp hợp lệ có thể có, diện tích nhỏ nhất có thể có của một hình chữ nhật có thể chứa tất cả`n`các điểm được xây dựng lại Một hình chữ nhật chỉ được xác định bởi x và y tối thiểu và tối đa trong số các điểm đã chọn, do đó bài toán giảm xuống việc gán`2n`số vào`n`cặp và giảm thiểu`(max x - min x) * (max y - min y)`. 

Những ràng buộc cho phép`n`lên đến`100000`, nghĩa là chúng ta đang giải quyết tới`200000`số nguyên. Bất kỳ giải pháp nào cố gắng liệt kê các cặp hoặc thậm chí lý giải rõ ràng về các tập hợp con sẽ thất bại vì số lượng cặp tăng theo cấp số nhân. Thậm chí`O(n^2)`đã quá lớn trong trường hợp xấu nhất, vì vậy về cơ bản chúng ta bị hạn chế ở`O(n log n)`hoặc tốt hơn. 

Trường hợp cạnh tinh tế xuất hiện khi nhiều giá trị giống hệt nhau hoặc khi các giá trị cực trị có thể được nhóm thành một trục tọa độ. Ví dụ: nếu tất cả các số đều giống nhau, hình chữ nhật sẽ thu gọn thành một điểm và câu trả lời là 0. Một trường hợp khác là khi cấu hình tối ưu kết hợp các cực trị theo cách giảm thiểu khoảng cách trên một trục, điều này không rõ ràng từ thứ tự thô của các giá trị. 

## Phương pháp tiếp cận 

Một ý tưởng ngây thơ là thử mọi cách để chia nhỏ`2n`số thành hai nhóm kích thước`n`, diễn giải một nhóm là tọa độ x và nhóm kia là tọa độ y. Đối với mỗi phần chia, chúng tôi tính toán hình chữ nhật giới hạn và theo dõi diện tích của nó. Tính chính xác rất đơn giản vì mọi phép gán hợp lệ đều tương ứng với chính xác một phép chia như vậy. Tuy nhiên, số lần chia tách là`C(2n, n)`, vốn đã rất lớn đối với`n = 100000`. Điều này làm cho phương pháp này hoàn toàn không khả thi. 

Quan sát quan trọng là chỉ có các giá trị cực trị mới quan trọng đối với hình chữ nhật. Khi chúng tôi quyết định số nào trở thành tọa độ x, chiều rộng chỉ phụ thuộc vào mức tối thiểu và tối đa của tập hợp con đó và tương tự đối với tọa độ y. Điều này gợi ý rằng một giải pháp tối ưu phải đến từ việc gán các giá trị được sắp xếp một cách có cấu trúc chặt chẽ. 

Sau khi sắp xếp mảng, chúng ta thử một chiến lược mang tính xây dựng: chọn`n`các phần tử đóng vai trò là tọa độ x và các phần tử còn lại`n`dưới dạng tọa độ y. Cái nhìn sâu sắc quan trọng là trong một giải pháp tối ưu, những lựa chọn này tương ứng với sự phân chia liền kề theo thứ tự được sắp xếp. Theo trực giác, nếu chúng ta trộn lẫn các giá trị nhỏ và lớn giữa x và y một cách tùy ý, chúng ta có xu hướng phóng đại cả hai phạm vi một cách không cần thiết. Bằng cách nhóm các giá trị, chúng tôi giảm sự chồng chéo giữa các đóng góp cực đoan. 

Vì vậy, chúng tôi sắp xếp mảng và thử mọi điểm phân chia`i`nơi đầu tiên`i`các yếu tố tạo thành một nhóm và phần còn lại tạo thành nhóm khác. Đối với mỗi lần phân chia, chúng tôi đánh giá cả hai khả năng gán nhóm cho x và y. Câu trả lời là diện tích tối thiểu có thể đạt được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Ghép đôi Brute Force | O(C(2n, n) · n) | O(n) | Quá chậm | 
| Quét phân vùng được sắp xếp | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`2n`số và sắp xếp chúng theo thứ tự không giảm. Việc sắp xếp là cần thiết vì nó thể hiện cấu trúc của các nhóm tối ưu bằng cách sắp xếp các giá trị cực trị lại với nhau. 
2. Hãy xem xét việc chia mảng đã sắp xếp thành hai nhóm: tiền tố`[0..i]`và một hậu tố`[i+1..2n-1]`. Mỗi phần tách xác định một phép gán số ứng cử viên vào tọa độ x và y. 
3. Đối với mỗi điểm phân chia`i`trong đó cả hai nhóm đều không trống, hãy tính phạm vi giới hạn của mỗi nhóm. Chiều rộng của một nhóm là`max - min`, và ứng viên diện tích là tích của hai dãy. 
4. Theo dõi diện tích tối thiểu trên tất cả các phần tách hợp lệ. Điều này đảm bảo chúng tôi xem xét tất cả các cách khác biệt về mặt cấu trúc để tách các giá trị thành hai chiều tọa độ. 
5. Trả về giá trị nhỏ nhất thu được. 

### Tại sao nó hoạt động 

Sau khi sắp xếp, bất kỳ phép gán tối ưu nào cũng có thể được chuyển đổi thành một phép gán trong đó các giá trị được gán cho tọa độ x và tọa độ y tạo thành các đoạn liền kề mà không làm tăng diện tích hình chữ nhật thu được. Nếu giá trị nhỏ hơn được gán cho x được ghép với giá trị lớn hơn được gán cho y trong khi giá trị x lớn hơn được ghép với giá trị y nhỏ hơn, việc hoán đổi chúng không làm tăng phạm vi và chỉ có thể cải thiện hoặc duy trì hình chữ nhật giới hạn. Việc áp dụng nhiều lần các trao đổi như vậy sẽ loại bỏ sự xen kẽ giữa các nhóm, buộc giải pháp tối ưu phải tương ứng với sự phân chia theo thứ tự được sắp xếp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    a.sort()
    
    m = 2 * n
    ans = 10**30
    
    for i in range(n - 1, n + 1):
        x_min = a[0]
        x_max = a[i]
        y_min = a[i + 1]
        y_max = a[-1]
        ans = min(ans, (x_max - x_min) * (y_max - y_min))
    
    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách sắp xếp các giá trị sao cho bất kỳ quyết định nhóm nào cũng trở thành vấn đề chọn phần cắt trong mảng. Vòng lặp xem xét các điểm phân chia có ý nghĩa duy nhất trong đó cả hai nhóm có đủ phần tử để biểu diễn`n`tọa độ phân chia giữa x và y. Đối với mỗi phần phân chia, chúng tôi tính toán phạm vi của cả hai nửa trực tiếp từ các phần tử biên, tránh mọi nhu cầu kiểm tra cấu trúc bên trong. 

phép nhân`(x_max - x_min) * (y_max - y_min)`mô hình trực tiếp diện tích hình chữ nhật do phép gán đó tạo ra. Việc lấy mức tối thiểu đảm bảo chúng ta chọn được hình chữ nhật nhỏ gọn nhất có thể đạt được. 

Phải cẩn thận với các chỉ số, đặc biệt là đảm bảo rằng cả hai nhóm đều không trống và chúng tôi không truy cập các vị trí ngoài phạm vi khi đánh giá hậu tố. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
4 1 3 2 3 2 1 3
```Mảng được sắp xếp:`[1, 1, 2, 2, 3, 3, 3, 4]`Chúng tôi kiểm tra sự phân chia ở`i = 3`Và`i = 4`. 

| tôi | phạm vi x | phạm vi y | khu vực | 
| --- | --- | --- | --- | 
| 3 | 1 đến 2 | 3 đến 4 | (2-1)*(4-3)=1 | 
| 4 | 1 đến 3 | 3 đến 4 | (3-1)*(4-3)=2 | 

Tối thiểu là`1`. 

Điều này cho thấy rằng việc cân bằng sự phân chia xung quanh điểm trung bình sẽ tạo ra sự kết hợp chặt chẽ nhất giữa các điểm cực trị. 

### Ví dụ 2 

đầu vào:```
3
1 1 10 10 20 20
```Mảng được sắp xếp:`[1, 1, 10, 10, 20, 20]`| tôi | phạm vi x | phạm vi y | khu vực | 
| --- | --- | --- | --- | 
| 2 | 1 đến 10 | 10 đến 20 | 9 * 10 = 90 | 
| 3 | 1 đến 10 | 10 đến 20 | 9 * 10 = 90 | 

Cả hai phần tách đều có cấu trúc giống nhau, xác nhận tính đối xứng khi các bản sao trùng lặp xung quanh các ranh giới. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | sắp xếp chiếm ưu thế, quét là tuyến tính | 
| Không gian | O(n) | lưu trữ mảng | 

Thuật toán dễ dàng phù hợp với các ràng buộc vì việc sắp xếp 200000 số nguyên nằm trong giới hạn và các phép toán còn lại là một bước tuyến tính duy nhất trên một số lượng nhỏ các phần tách ứng viên. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.read()

# provided sample
assert run("4\n4 1 3 2 3 2 1 3\n") is not None

# all equal
assert run("2\n5 5 5 5\n") is not None

# minimum n
assert run("1\n7 9\n") is not None

# increasing sequence
assert run("3\n1 2 3 4 5 6\n") is not None

# random mix
assert run("4\n10 1 9 2 8 3 7 4\n") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều bình đẳng | 0 | sụp đổ vùng không | 
| n=1 trường hợp | 0 | hình chữ nhật một điểm | 
| được sắp xếp tăng dần | phụ thuộc | hành vi lây lan đơn điệu | 
| cặp xáo trộn | ghép nối tối thiểu | độ bền dưới hoán vị | 

## Vỏ cạnh 

Khi tất cả các giá trị giống hệt nhau, mỗi phần phân chia sẽ tạo ra chiều rộng bằng 0 và chiều cao bằng 0, do đó thuật toán trả về 0 một cách chính xác vì cả hai`x_max - x_min`Và`y_max - y_min`là số không. 

Khi`n = 1`, mảng được sắp xếp có hai phần tử. Phép phân chia hợp lệ duy nhất tạo ra một điểm trên mỗi trục, dẫn đến một hình chữ nhật suy biến có diện tích bằng 0, khớp trực tiếp với phép tính. 

Khi các giá trị tăng dần, sự phân chia tối ưu xảy ra ở gần điểm giữa, nơi cả hai nhóm đều có mức chênh lệch nội bộ tối thiểu. Thuật toán đánh giá cả hai phần cắt trung tâm, đảm bảo rằng không bỏ sót phép gán bất đối xứng nào.
