---
title: "CF 105692G - Tìm cực đại thứ hai"
description: "Nhiệm vụ là trích xuất thông tin thứ tự từ một tập hợp các số: cho một chuỗi, chúng ta muốn xác định không chỉ giá trị lớn nhất mà còn cả giá trị ngay bên dưới nó trong thứ tự khi bỏ qua các giá trị trùng lặp."
date: "2026-06-26T08:09:06+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105692
codeforces_index: "G"
codeforces_contest_name: "Baozii Cup 1"
rating: 0
weight: 105692
solve_time_s: 34
verified: true
draft: false
---

[CF 105692G - Tìm mức tối đa thứ hai](https://codeforces.com/problemset/problem/105692/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 34s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ là trích xuất thông tin thứ tự từ một tập hợp các số: cho một chuỗi, chúng ta muốn xác định không chỉ giá trị lớn nhất mà còn cả giá trị ngay bên dưới nó trong thứ tự khi bỏ qua các giá trị trùng lặp. 

Cụ thể hơn, hãy tưởng tượng bạn đang nhìn vào một danh sách các số nguyên và liên tục hỏi số nào lớn nhất. Khi đã biết điều đó, về mặt khái niệm, bạn loại bỏ tất cả các lần xuất hiện của số lớn nhất đó và đặt lại câu hỏi tương tự. Câu trả lời cho câu hỏi thứ hai là cái mà chúng ta gọi là mức tối đa thứ hai. 

Đầu vào có thể được hiểu là một mảng số nguyên. Đầu ra là một số nguyên duy nhất biểu thị giá trị khác biệt lớn thứ hai trong mảng đó. Nếu mọi phần tử đều giống hệt nhau hoặc nếu chỉ có một giá trị duy nhất thì không có giá trị tối đa thứ hai hợp lệ để báo cáo và đầu ra phải phản ánh sự vắng mặt đó. 

Từ góc độ ràng buộc, giới hạn trên tự nhiên cho các vấn đề như vậy thường lên tới 10^5 phần tử trở lên. Điều đó ngay lập tức loại trừ bất kỳ giải pháp nào so sánh tất cả các cặp phần tử hoặc sắp xếp liên tục các mảng con cho mỗi truy vấn. Cách tiếp cận O(n^2) sẽ thực hiện khoảng 10^10 thao tác trong trường hợp xấu nhất và sẽ không phù hợp với giới hạn thời gian thông thường. Ngay cả giải pháp O(n log n) thông qua sắp xếp cũng có thể chấp nhận được, nhưng nó quá mức cần thiết vì cấu trúc của bài toán chỉ yêu cầu theo dõi hai giá trị. 

Trường hợp cạnh tinh vi xuất hiện khi mảng chứa các giá trị trùng lặp tối đa nhưng không tồn tại giá trị phân biệt thứ hai. Ví dụ, đầu vào`[5, 5, 5]`có tối đa`5`, nhưng không có mức tối đa thứ hai. Cách triển khai đơn giản chỉ chọn phần tử thứ hai sau khi sắp xếp mà không loại bỏ các phần tử trùng lặp sẽ xuất ra không chính xác`5`, vi phạm yêu cầu về tính khác biệt. 

Một trường hợp cạnh khác phát sinh khi cực đại xảy ra một lần và cực đại thứ hai được lặp lại nhiều lần, chẳng hạn như`[10, 3, 3, 3]`. Một giải pháp đúng phải đảm bảo nó không vô tình coi các vị trí quét trùng lặp là các ứng cử viên riêng biệt; nó chỉ nên quan tâm đến sự chuyển đổi giá trị chứ không phải tần số. 

## Phương pháp tiếp cận 

Ý tưởng đơn giản nhất là sắp xếp mảng theo thứ tự giảm dần và chọn phần tử đầu tiên làm giá trị lớn nhất và quét về phía trước cho đến khi tìm thấy giá trị khác biệt nhỏ hơn. Điều này đúng vì việc sắp xếp nhóm các phần tử bằng nhau lại với nhau, do đó vị trí đầu tiên nơi giá trị thay đổi tương ứng chính xác với giá trị lớn nhất thứ hai. Hạn chế là chi phí sắp xếp là O(n log n) và bài toán thực tế không yêu cầu sắp xếp đầy đủ mà chỉ yêu cầu hai giá trị phân biệt cao nhất. 

Một cách tiếp cận hiệu quả hơn đến từ việc nhận thấy rằng chúng ta chỉ quan tâm đến hai giá trị trong khi quét mảng một lần. Thay vì xây dựng trật tự toàn cầu, chúng tôi duy trì hai biến số đang diễn ra: giá trị lớn nhất được thấy cho đến nay và giá trị riêng biệt lớn thứ hai được thấy cho đến nay. Mỗi phần tử mới chỉ cần được so sánh với hai phần tử này, điều này làm giảm vấn đề xuống còn một lần. 

Quan sát quan trọng là bất cứ khi nào chúng ta gặp một giá trị lớn hơn mức tối đa hiện tại thì mức tối đa trước đó sẽ trở thành ứng cử viên tối đa thứ hai. Khi giá trị nằm hoàn toàn giữa mức tối đa hiện tại và mức tối đa thứ hai hiện tại, nó sẽ cập nhật mức tối đa thứ hai. Mọi thứ khác có thể được bỏ qua. 

Điều này làm giảm toàn bộ vấn đề về thời gian tuyến tính với không gian bổ sung không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Sắp xếp | O(n log n) | O(1) hoặc O(n) | Được chấp nhận nhưng không cần thiết | 
| Theo dõi một lượt | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì hai biến trong một lần truyền qua mảng:`max1`là giá trị lớn nhất được thấy cho đến nay và`max2`là giá trị khác biệt lớn thứ hai. 

1. Khởi tạo cả hai`max1`Và`max2`là không xác định (hoặc vô cực âm tùy thuộc vào việc triển khai). Điều này thể hiện rằng chúng tôi chưa thấy bất kỳ giá trị nào. 
2. Đọc từng số`x`từ mảng từng cái một. 
3. Nếu`x`lớn hơn`max1`, sau đó`x`trở thành giá trị lớn nhất mới. Trước đó`max1`(nếu nó tồn tại) được chuyển xuống để trở thành mới`max2`. Bước này bảo tồn hai giá trị riêng biệt tốt nhất được thấy cho đến nay. 
4. Ngược lại nếu`x`là nghiêm ngặt giữa`max1`Và`max2`, sau đó`x`cải thiện ứng cử viên tốt thứ hai và thay thế`max2`. Chúng tôi bỏ qua sự bình đẳng với`max1`ở đây vì các bản sao không tạo ra mức tối đa khác biệt mới. 
5. Nếu không thì đừng làm gì cả. Giá trị này có thể trùng lặp với thứ mà chúng ta đã tính đến hoặc quá nhỏ nên không quan trọng. 
6. Sau khi xử lý tất cả các phần tử, kiểm tra xem`max2`đã từng được gán một giá trị hợp lệ. Nếu không thì không tồn tại mức tối đa khác biệt thứ hai. 

Lý do điều này có tác dụng là tại bất kỳ thời điểm nào trong quá trình quét,`max1`được đảm bảo là phần tử lớn nhất trong số tất cả các giá trị được xử lý và`max2`là giá trị lớn nhất nhỏ hơn rất nhiều so với`max1`giữa những phần tử giống nhau. Bất kỳ phần tử mới nào cũng chỉ cần cạnh tranh với hai phần tóm tắt này vì tất cả cấu trúc nhỏ hơn đều không liên quan đến việc xác định hai giá trị riêng biệt hàng đầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    arr = list(map(int, input().split()))
    
    max1 = None
    max2 = None
    
    for x in arr:
        if max1 is None or x > max1:
            max2 = max1
            max1 = x
        elif x != max1:
            if max2 is None or x > max2:
                max2 = x
    
    if max2 is None:
        print("NO SECOND MAX")
    else:
        print(max2)

if __name__ == "__main__":
    solve()
```Việc triển khai phản ánh trực tiếp bất biến hai biến. Nhánh đầu tiên xử lý việc thăng hạng lên vị trí tối đa và hạ cấp chính xác mức tối đa trước đó. Nhánh thứ hai đảm bảo rằng chỉ những giá trị hoàn toàn khác với mức tối đa hiện tại mới có thể ảnh hưởng`max2`, trong đó thực thi sự khác biệt. Kiểm tra cuối cùng phân biệt giữa việc có mức tối đa thứ hai hợp lệ và việc có tất cả các giá trị giống hệt nhau. 

Một cạm bẫy phổ biến là cho phép bình đẳng với`max1`để cập nhật`max2`, điều này sẽ coi các bản sao của mức tối đa là mức tối đa thứ hai hợp lệ không chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6
5 1 5 3 2 4
```| Bước | x | tối đa1 | tối đa2 | 
| --- | --- | --- | --- | 
| 1 | 5 | 5 | Không có | 
| 2 | 1 | 5 | 1 | 
| 3 | 5 | 5 | 1 | 
| 4 | 3 | 5 | 3 | 
| 5 | 2 | 5 | 3 | 
| 6 | 4 | 5 | 4 | 

Dấu vết cho thấy làm thế nào`max1`ổn định lúc 5 giờ sớm, trong khi`max2`liên tục được cải thiện khi các ứng cử viên lớn hơn xuất hiện bên dưới nó. Câu trả lời cuối cùng là 4, là giá trị lớn nhất nhỏ hơn 5. 

### Ví dụ 2 

đầu vào:```
4
7 7 7 7
```| Bước | x | tối đa1 | tối đa2 | 
| --- | --- | --- | --- | 
| 1 | 7 | 7 | Không có | 
| 2 | 7 | 7 | Không có | 
| 3 | 7 | 7 | Không có | 
| 4 | 7 | 7 | Không có | 

Trường hợp này chứng minh tại sao sự khác biệt lại quan trọng. Mặc dù mảng chứa nhiều phần tử nhưng không có giá trị nào nhỏ hơn giá trị tối đa, vì vậy`max2`không bao giờ được giao. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử được xử lý chính xác một lần với các phép so sánh theo thời gian không đổi | 
| Không gian | O(1) | Chỉ có hai biến được duy trì bất kể kích thước đầu vào | 

Quét tuyến tính là đủ cho các ràng buộc lên tới ít nhất 10^5 hoặc 10^6 phần tử, duy trì tốt trong giới hạn thời gian thông thường cho khoảng thời gian thực thi 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# custom cases

# single element
assert run("1\n10\n") == "NO SECOND MAX", "single element"

# all equal
assert run("5\n3 3 3 3 3\n") == "NO SECOND MAX", "all equal"

# simple increasing
assert run("5\n1 2 3 4 5\n") == "4", "increasing"

# duplicates of max with valid second
assert run("6\n9 1 9 2 9 3\n") == "3", "duplicates max"

# two elements
assert run("2\n100 50\n") == "50", "two elements"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | KHÔNG CÓ TỐI ĐA THỨ HAI | ranh giới tối thiểu | 
| tất cả đều bình đẳng | KHÔNG CÓ TỐI ĐA THỨ HAI | mảng chỉ trùng lặp | 
| ngày càng tăng | 4 | đặt hàng bình thường | 
| trùng lặp tối đa | 3 | xử lý lặp lại tối đa | 
| hai yếu tố | 50 | trường hợp không tầm thường nhỏ nhất | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi tất cả các giá trị giống hệt nhau. Trong tình huống này, thuật toán không bao giờ gán`max2`, vì mọi so sánh đều khớp`max1`hoặc không tạo ra được một ứng cử viên nhỏ hơn. Kiểm tra cuối cùng sẽ phát hiện chính xác điều này và tạo ra đầu ra “không có mức tối đa thứ hai”. 

Một trường hợp cạnh khác xảy ra khi giá trị lớn nhất xuất hiện muộn trong mảng. Ví dụ, trong`[1, 2, 3, 100, 50]`, thuật toán phải cập nhật chính xác cả hai`max1`và thiết lập lại`max2`khi gặp 100, đảm bảo rằng các ứng cử viên trước đó không bị bảo toàn không chính xác là tốt nhất thứ hai so với mức tối đa mới. Bước giảm hạng ở nhánh đầu tiên đảm bảo hành vi này bằng cách chuyển mức tối đa trước đó sang vị trí thứ hai bất cứ khi nào tìm thấy mức tối đa mới.
