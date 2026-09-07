---
title: "CF 104555A - Công viên giải trí phiêu lưu"
description: "Chúng tôi được cung cấp một công viên giải trí nhỏ với danh sách các trò chơi cố định, mỗi trò chơi đều có yêu cầu về chiều cao tối thiểu. Carlitos có chiều cao cố định và anh ta chỉ có thể tham gia một chuyến đi nếu chiều cao của anh ta ít nhất bằng yêu cầu của chuyến đi đó."
date: "2026-06-30T08:46:09+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104555
codeforces_index: "A"
codeforces_contest_name: "2023-2024 ICPC Brazil Subregional Programming Contest"
rating: 0
weight: 104555
solve_time_s: 76
verified: true
draft: false
---

[CF 104555A - Cuộc phiêu lưu trong công viên giải trí](https://codeforces.com/problemset/problem/104555/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 16s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một công viên giải trí nhỏ với danh sách các trò chơi cố định, mỗi trò chơi đều có yêu cầu về chiều cao tối thiểu. Carlitos có chiều cao cố định và anh ta chỉ có thể tham gia một chuyến đi nếu chiều cao của anh ta ít nhất bằng yêu cầu của chuyến đi đó. Nhiệm vụ chỉ đơn giản là đếm xem có bao nhiêu chuyến đi thỏa mãn điều kiện này. 

Bạn có thể coi điều này giống như việc so sánh một số với từng phần tử của một mảng nhỏ và đếm xem có bao nhiêu phần tử không lớn hơn số đó. 

Những hạn chế là cực kỳ nhỏ. Số lượng chuyến đi tối đa là 6 và độ cao được giới hạn trong một phạm vi hẹp. Điều này ngay lập tức cho chúng ta biết rằng ngay cả cách tiếp cận trực tiếp nhất, kiểm tra từng chuyến đi riêng lẻ, cũng nhanh chóng một cách tầm thường. Bất kỳ thuật toán nào lặp lại trên tất cả các chuyến đi với số lần không đổi sẽ đủ hiệu quả. 

Không có trường hợp ẩn phức tạp nào liên quan đến việc đặt hàng, trùng lặp hoặc đầu vào lớn. Trường hợp lợi thế duy nhất có ý nghĩa là khi Carlitos thấp hơn tất cả các cuộc đua, trong trường hợp đó câu trả lời là 0 hoặc khi anh ta đủ cao cho tất cả các cuộc đua, trong trường hợp đó câu trả lời là N. 

Một lỗi phổ biến trong các vấn đề tương tự là hiểu sai hướng của điều kiện, ví dụ như tính số chuyến đi trong đó yêu cầu lớn hơn chiều cao thay vì nhỏ hơn hoặc bằng. Một vấn đề tiềm ẩn khác là quên đi sự bình đẳng, vì những chuyến đi có yêu cầu chính xác bằng chiều cao của Carlitos đều được phép. 

## Phương pháp tiếp cận 

Cách trực tiếp nhất để giải quyết vấn đề là kiểm tra từng chuyến đi một và kiểm tra xem Carlitos có đáp ứng yêu cầu về chiều cao hay không. Đối với mỗi chuyến đi, chúng tôi so sánh chiều cao cần thiết của nó với chiều cao của Carlitos và tăng bộ đếm nếu điều kiện được thỏa mãn. 

Điều này hiệu quả vì mỗi chuyến đi đều độc lập với những chuyến đi khác. Không có sự tương tác giữa các chuyến đi, không có ràng buộc về thứ tự và không có cấu trúc tối ưu hóa để khai thác. Cách tiếp cận vũ phu đã tối ưu vì kích thước đầu vào không đổi. 

Nếu chúng ta tưởng tượng một phiên bản tổng quát hơn trong đó N có thể lớn, chẳng hạn lên tới 100.000, thì ý tưởng tương tự vẫn hoạt động trong O(N), đó là quét tuyến tính. Việc sắp xếp hoặc tìm kiếm nhị phân sẽ không cần thiết vì chúng tôi không truy vấn nhiều lần hoặc cần cấu trúc tiền tố mà chỉ đếm các so sánh đơn giản. 

Brute-force hoạt động vì nó mã hóa trực tiếp định nghĩa của vấn đề, nhưng trong những ràng buộc lớn hơn, nó sẽ chỉ bị coi là ngây thơ nếu lặp lại nhiều lần. Đây đã là giải pháp cuối cùng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (quét tất cả các chuyến đi) | O(N) | O(1) | Đã chấp nhận | 
| Tối ưu (quét cùng) | O(N) | O(1) | Đã chấp nhận | 

Trong bài toán này, cả hai hàng đều mô tả cùng một thuật toán vì không thể hoặc không cần tối ưu hóa thêm nữa. 

## Hướng dẫn thuật toán 

1. Đọc số chuyến đi N và chiều cao H của Carlitos. Những thông số này xác định kích thước của danh sách và ngưỡng hợp lệ. 
2. Đọc danh sách các yêu cầu về chiều cao khi đi xe A. Mỗi giá trị thể hiện chiều cao tối thiểu cần thiết để tham gia chuyến đi đó. 
3. Khởi tạo bộ đếm về 0. Điều này sẽ tích lũy số lượng chuyến đi hợp lệ. 
4. Lặp lại từng yêu cầu trong A. 
5. Đối với mỗi yêu cầu, hãy kiểm tra xem nó có nhỏ hơn hoặc bằng H hay không. Điều kiện này xác định liệu Carlitos có thể tham gia chuyến đi một cách an toàn hay không. 
6. Nếu điều kiện được giữ, hãy tăng bộ đếm. 
7. Sau khi xử lý tất cả các chuyến đi, xuất bộ đếm cuối cùng. 

Ý tưởng chính là mỗi phép so sánh quyết định tính đủ điều kiện một cách độc lập, vì vậy chúng ta không cần lưu trữ hoặc chuyển đổi mảng theo bất kỳ cách nào. 

### Tại sao nó hoạt động

Mỗi chuyến đi đóng góp 1 hoặc 0 cho câu trả lời cuối cùng chỉ tùy thuộc vào việc yêu cầu của nó có nằm trong giới hạn của Carlitos hay không. Vì các quyết định này là độc lập và loại trừ lẫn nhau trên mỗi chuyến đi nên việc tổng hợp các chỉ số này trên tất cả các chuyến đi sẽ tạo ra tổng số chính xác. Thuật toán tính toán hiệu quả số phần tử trong mảng thỏa mãn một vị từ đơn giản, chính xác là định nghĩa của kết quả được yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n, h = map(int, input().split())
    a = list(map(int, input().split()))
    
    count = 0
    for x in a:
        if x <= h:
            count += 1
    
    print(count)

if __name__ == "__main__":
    main()
```Giải pháp bắt đầu bằng cách đọc đầu vào ở định dạng trực tiếp nhất. Danh sách các yêu cầu về chuyến đi được lưu dưới dạng mảng vì chúng ta chỉ cần lặp lại một lần. 

Vòng lặp cốt lõi là toàn bộ thuật toán: mỗi phần tử được kiểm tra theo ngưỡng H. Điều kiện`x <= h`là rất quan trọng, vì phải bao gồm sự bình đẳng. Một sai lầm nhỏ sẽ là đảo ngược sự so sánh này hoặc sử dụng bất đẳng thức nghiêm ngặt, cả hai đều sẽ tạo ra số đếm không chính xác trong các trường hợp biên. 

Cuối cùng, bộ đếm tích lũy được in. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
1 100
100
```| Bước | H | Chuyến đi hiện tại | Điều kiện (x ≤ H) | Quầy | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | 100 | - | - | 0 | 
| 1 | 100 | 100 | đúng | 1 | 

Chuyến đi đơn có yêu cầu chính xác bằng chiều cao của Carlitos nên hợp lệ. Câu trả lời cuối cùng là 1. 

Ví dụ này cho thấy tầm quan trọng của việc đưa sự bình đẳng vào điều kiện. 

### Mẫu 2 

đầu vào:```
6 120
200 90 100 123 120 169
```| Bước | H | Chuyến đi hiện tại | Điều kiện (x ≤ H) | Quầy | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | 120 | - | - | 0 | 
| 1 | 120 | 200 | sai | 0 | 
| 2 | 120 | 90 | đúng | 1 | 
| 3 | 120 | 100 | đúng | 2 | 
| 4 | 120 | 123 | sai | 2 | 
| 5 | 120 | 120 | đúng | 3 | 
| 6 | 120 | 169 | sai | 3 | 

Chỉ có ba chuyến đi thỏa mãn điều kiện ràng buộc nên đầu ra là 3. 

Dấu vết này cho thấy việc đặt hàng không hề quan trọng; mỗi phần tử được xử lý độc lập. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Chúng tôi quét mỗi chuyến đi chính xác một lần và thực hiện một so sánh cho mỗi phần tử | 
| Không gian | O(1) | Chỉ có một bộ đếm được lưu trữ; lưu trữ đầu vào không phải là tính toán thêm | 

Cho rằng N ≤ 6, điều này chạy ngay lập tức ngay cả khi có chi phí hoạt động. Giải pháp này thấp hơn nhiều so với bất kỳ giới hạn thực tế nào và thậm chí một hạn chế lớn hơn nhiều vẫn có hiệu quả. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline
    
    n, h = map(int, input().split())
    a = list(map(int, input().split()))
    
    count = 0
    for x in a:
        if x <= h:
            count += 1
    return str(count)

# provided samples
assert run("1 100\n100\n") == "1", "sample 1"
assert run("6 120\n200 90 100 123 120 169\n") == "3", "sample 2"

# custom cases
assert run("3 150\n90 150 200\n") == "2", "boundary equality and mixed values"
assert run("4 100\n101 102 103 104\n") == "0", "no accessible rides"
assert run("5 200\n90 100 110 120 130\n") == "5", "all accessible rides"
assert run("1 90\n90\n") == "1", "minimum boundary case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Giá trị hỗn hợp xung quanh ngưỡng | 2 | Xử lý đúng sự bình đẳng và lọc | 
| Tất cả trên ngưỡng | 0 | Không có trường hợp đi xe hợp lệ | 
| Tất cả đều dưới ngưỡng | 5 | Trường hợp chấp nhận hoàn toàn | 
| Giá trị biên đơn | 1 | Độ chính xác đầu vào tối thiểu | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi tất cả các yêu cầu về xe đều vượt quá chiều cao của Carlitos. 

đầu vào:```
3 100
150 120 110
```Thuật toán kiểm tra từng giá trị: 

150 > 100 cho kết quả sai, 120 > 100 cho kết quả sai, 110 > 100 cho kết quả sai, do đó bộ đếm vẫn bằng 0. Đầu ra là 0, phù hợp với mong đợi. 

Một trường hợp khác là khi tất cả các chuyến đi đều có chiều cao chính xác. 

đầu vào:```
3 120
120 120 120
```Mỗi so sánh thỏa mãn`x <= H`, do đó bộ đếm tăng gấp ba lần. Đầu ra cuối cùng là 3. Điều này xác nhận rằng đẳng thức được xử lý chính xác và không cần viết hoa đặc biệt.
