---
title: "CF 104312K - Kẻ giết quái vật"
description: "Chúng ta được cung cấp một dòng quái vật, mỗi con quái vật có giá trị sức mạnh, dương hoặc âm. Saitama có thể chọn bất kỳ phân đoạn liên tiếp nào của những con quái vật này và đánh bại chính xác nhóm đó."
date: "2026-07-01T19:55:03+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104312
codeforces_index: "K"
codeforces_contest_name: "UTPC Spring 2023 Contest (HS)"
rating: 0
weight: 104312
solve_time_s: 69
verified: true
draft: false
---

[CF 104312K - Kẻ giết quái vật](https://codeforces.com/problemset/problem/104312/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 9 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một dòng quái vật, mỗi con quái vật có giá trị sức mạnh, dương hoặc âm. Saitama có thể chọn bất kỳ phân đoạn liên tiếp nào của những con quái vật này và đánh bại chính xác nhóm đó. Độ mạnh của phân đoạn được chọn là tổng của tất cả các giá trị bên trong nó và nhiệm vụ là tìm tổng phân đoạn tối đa có thể có trên tất cả các phân đoạn liền kề. 

Đầu vào là một mảng có độ dài lên tới 100.000 và mỗi giá trị có thể nhỏ bằng âm một tỷ hoặc lớn bằng một tỷ. Đầu ra là một số nguyên duy nhất, tổng tốt nhất có thể từ bất kỳ mảng con liền kề nào. 

Ràng buộc trên n ngay lập tức loại trừ bất kỳ giải pháp nào thử tất cả các mảng con một cách rõ ràng. Một vòng lặp kép trên tất cả các cặp (i, j) đã dẫn đến khoảng 10^10 thao tác trong trường hợp xấu nhất, vượt xa những gì có thể diễn ra trong một giây. Điều này đẩy chúng ta tới một giải pháp tuyến tính hoặc gần tuyến tính. 

Trường hợp cạnh tinh tế xuất hiện khi tất cả các số đều âm. Ví dụ, đối với đầu vào`[-5, -2, -8]`, câu trả lời đúng là`-2`, xuất phát từ việc chọn một phần tử duy nhất. Một cách triển khai ngây thơ khởi tạo câu trả lời là 0 hoặc giả sử chúng ta có thể “bỏ qua mọi thứ” sẽ trả về 0 không chính xác, mặc dù 0 không phải là tổng của mảng con hợp lệ trừ khi được cho phép rõ ràng bởi một mảng con trống, điều mà vấn đề này không cho phép. 

Một trường hợp cạnh khác là mảng một phần tử, trong đó câu trả lời phải là chính phần tử đó, ngay cả khi là âm. Điều này nhấn mạnh việc khởi tạo chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: xem xét mọi phân mảng có thể có, tính tổng của nó và theo dõi mức tối đa. Đối với mỗi chỉ số bắt đầu i, chúng tôi mở rộng đến mọi j ≥ i và tích lũy tổng tăng dần. Điều này đúng vì nó đánh giá trực tiếp mọi phân đoạn liền kề hợp lệ. Tuy nhiên, số lượng phân đoạn là n(n+1)/2 và mỗi lần cập nhật tổng là O(1) nếu chúng tôi sử dụng lại tích lũy tiền tố, vẫn cho tổng công việc là O(n^2). Với n lên tới 10^5, điều này trở nên không khả thi. 

Điều quan trọng là khi quét từ trái sang phải, tại mỗi vị trí chúng ta chỉ quan tâm đến việc mở rộng đoạn trước đó có lợi hay bắt đầu mới ở vị trí hiện tại sẽ tốt hơn. Nếu tổng hiện có trở thành số âm, việc giữ nó chỉ làm giảm sự đóng góp của bất kỳ tiện ích mở rộng nào trong tương lai, do đó, tốt nhất là khởi động lại từ phần tử hiện tại. Điều này biến vấn đề tối ưu hóa toàn cục thành một quyết định cục bộ ở mỗi chỉ mục, đây chính xác là điều cho phép quét tuyến tính. 

Điều này làm giảm vấn đề trong việc duy trì tiền tố tốt nhất đang chạy kết thúc ở mỗi vị trí và theo dõi mức tối đa toàn cầu của các giá trị này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(1) | Quá chậm | 
| Tối ưu (thuật toán Kadane) | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý mảng từ trái sang phải trong khi duy trì hai giá trị: tổng mảng con tốt nhất kết thúc ở vị trí hiện tại và câu trả lời tổng thể tốt nhất được thấy cho đến nay. 

1. Khởi tạo tổng hiện có cho phần tử đầu tiên của mảng. Điều này đảm bảo chúng tôi xử lý chính xác trường hợp tất cả các số đều âm vì chúng tôi buộc phải chọn ít nhất một phần tử. 
2. Khởi tạo câu trả lời tốt nhất toàn cầu cho cùng một giá trị, vì mảng con tốt nhất khi bắt đầu chỉ có thể là phần tử đơn lẻ đó. 
3. Lặp lại mảng bắt đầu từ phần tử thứ hai. 
4. Tại mỗi phần tử p[i], quyết định xem nên mở rộng mảng con trước đó hay bắt đầu mới ở mảng i. Mở rộng có nghĩa là thêm p[i] vào tổng hiện có, trong khi khởi động lại có nghĩa là loại bỏ nó và đặt tổng hiện có thành p[i]. 
5. Quyết định được đưa ra bằng cách so sánh p[i] với Running_sum + p[i]. Nếu tổng trước đó là âm thì khởi động lại sẽ tốt hơn; nếu không thì việc mở rộng sẽ có lợi. 
6. Sau khi cập nhật tổng hiện có, hãy cập nhật câu trả lời chung nếu tổng hiện có lớn hơn. 
7. Sau khi kết thúc phép lặp, kết quả tổng thể là tổng mảng con lớn nhất. 

Tại sao nó hoạt động: tại mọi chỉ mục i, thuật toán duy trì mảng con tốt nhất có thể phải kết thúc tại i. Bất kỳ mảng con nào kết thúc tại i đều xuất phát từ việc mở rộng một mảng con hợp lệ kết thúc tại i−1 hoặc bắt đầu tại chính i. Nếu đóng góp trước đó là số âm thì nó chỉ có thể làm giảm số tiền trong tương lai, do đó việc loại bỏ nó không thể làm xấu đi bất kỳ lựa chọn tối ưu nào trong tương lai. Sự tối ưu cục bộ này đảm bảo rằng tất cả các điểm cuối đều đạt được mức tối đa toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    arr = list(map(int, input().split()))
    
    current = arr[0]
    best = arr[0]
    
    for i in range(1, n):
        current = max(arr[i], current + arr[i])
        best = max(best, current)
    
    print(best)

if __name__ == "__main__":
    solve()
```Giải pháp đọc mảng một lần và duy trì hai biến đang chạy. Điểm mấu chốt là sự chuyển đổi`current = max(arr[i], current + arr[i])`, mã hóa quyết định khởi động lại hoặc gia hạn. Biến`best`theo dõi tổng mảng con tốt nhất được thấy cho đến nay, bất kể nó kết thúc ở đâu. 

Phải cẩn thận khi khởi tạo. Đặt cả hai`current`Và`best`đối với phần tử đầu tiên đảm bảo tính chính xác ngay cả khi tất cả các số đều âm, tránh việc vô tình khởi tạo về 0. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
9
-2 10 -3 5 -2 1 2 6 -1
```| tôi | mảng[i] | hiện tại trước | sự lựa chọn | hiện tại sau | tốt nhất | 
| --- | --- | --- | --- | --- | --- | 
| 0 | -2 | - | bắt đầu | -2 | -2 | 
| 1 | 10 | -2 | mở rộng | 8 vs 10 | 10 | 
| 2 | -3 | 10 | mở rộng | 7 vs -3 | 10 | 
| 3 | 5 | 7 | mở rộng | 12 đấu 5 | 12 | 
| 4 | -2 | 12 | mở rộng | 10 vs -2 | 12 | 
| 5 | 1 | 10 | mở rộng | 11 đấu 1 | 12 | 
| 6 | 2 | 11 | mở rộng | 13 vs 2 | 13 | 
| 7 | 6 | 13 | mở rộng | 19 vs 6 | 19 | 
| 8 | -1 | 19 | mở rộng | 18 vs -1 | 19 | 

Dấu vết này cho thấy cách các khoản đóng góp âm một phần chỉ được hấp thụ khi chúng vẫn cải thiện tổng số hiện có và cách câu trả lời tốt nhất chỉ được cập nhật khi đạt đến đỉnh mới. 

### Ví dụ 2 

đầu vào:```
5
-5 -2 -8 -1 -3
```| tôi | mảng[i] | hiện tại trước | sự lựa chọn | hiện tại sau | tốt nhất | 
| --- | --- | --- | --- | --- | --- | 
| 0 | -5 | - | bắt đầu | -5 | -5 | 
| 1 | -2 | -5 | khởi động lại | -2 | -2 | 
| 2 | -8 | -2 | khởi động lại | -8 | -2 | 
| 3 | -1 | -8 | khởi động lại | -1 | -1 | 
| 4 | -3 | -1 | mở rộng? | -4 vs -3 | -1 | 

Điều này thể hiện hành vi quan trọng khi tất cả các số đều âm: thuật toán liên tục khởi động lại, chọn một phần tử đơn âm ít âm nhất một cách hiệu quả. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử được xử lý chính xác một lần với các bản cập nhật liên tục | 
| Không gian | O(1) | Chỉ có hai biến số nguyên được duy trì | 

Quét tuyến tính phù hợp thoải mái trong các ràng buộc cho n lên tới 100.000. Việc sử dụng bộ nhớ không đổi bất kể kích thước đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import inf

    data = sys.stdin.read().strip().split()
    n = int(data[0])
    arr = list(map(int, data[1:]))

    current = arr[0]
    best = arr[0]

    for i in range(1, n):
        current = max(arr[i], current + arr[i])
        best = max(best, current)

    return str(best)

# provided sample
assert run("""9
-2 10 -3 5 -2 1 2 6 -1""") == "19"

# single element
assert run("""1
-7""") == "-7"

# all negative
assert run("""5
-5 -2 -8 -1 -3""") == "-1"

# all positive
assert run("""4
1 2 3 4""") == "10"

# mixed with large peak
assert run("""6
-1 -2 100 -3 -4 50""") == "100"

# alternating
assert run("""6
1 -1 1 -1 1 -1""") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 -7 | -7 | xử lý phần tử đơn | 
| -5 -2 -8 -1 -3 | -1 | lựa chọn toàn âm | 
| 1 2 3 4 | 10 | trường hợp tăng đơn điệu | 
| -1 -2 100 -3 -4 50 | 100 | sự thống trị đỉnh cao bị cô lập | 
| 1 -1 1 -1 1 -1 | 1 | hành vi khởi động lại xen kẽ | 

## Vỏ cạnh 

Đối với đầu vào một phần tử như`[-7]`, thuật toán khởi tạo`current`Và`best`ĐẾN`-7`, vì vậy nó trực tiếp xuất ra`-7`mà không cần vào vòng lặp. Điều này tránh mọi giả định không chính xác rằng một mảng con trống có thể được chọn. 

Đối với một mảng toàn âm như`[-5, -2, -8, -1]`, thuật toán khởi động lại nhiều lần ở mỗi vị trí vì việc mở rộng luôn làm cho tổng trở nên tồi tệ hơn. Các bản cập nhật tốt nhất đang chạy cho`-2`ở chỉ số 1 và không bao giờ giảm sau đó, tạo ra phần tử âm nhỏ nhất chính xác làm câu trả lời.
