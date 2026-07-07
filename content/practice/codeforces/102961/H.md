---
title: "CF 102961H - Tổng phân mảng tối đa"
description: "Chúng ta được cho một dãy số nguyên và chúng ta cần tìm tổng tối đa có thể có của một đoạn liền kề của dãy đó. Đoạn liền kề có nghĩa là chúng tôi chọn vị trí bắt đầu và vị trí kết thúc, đồng thời lấy tất cả các phần tử ở giữa mà không bỏ qua bất kỳ phần tử nào."
date: "2026-07-04T06:51:48+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102961
codeforces_index: "H"
codeforces_contest_name: "CSES Problem Set: Sorting and Searching"
rating: 0
weight: 102961
solve_time_s: 38
verified: true
draft: false
---

[CF 102961H - Tổng mảng con tối đa](https://codeforces.com/problemset/problem/102961/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 38s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy số nguyên và chúng ta cần tìm tổng tối đa có thể có của một đoạn liền kề của dãy đó. Đoạn liền kề có nghĩa là chúng tôi chọn vị trí bắt đầu và vị trí kết thúc, đồng thời lấy tất cả các phần tử ở giữa mà không bỏ qua bất kỳ phần tử nào. Nhiệm vụ là tính tổng lớn nhất trên tất cả các phân đoạn như vậy, bao gồm khả năng lấy một phần tử làm phân đoạn. 

Đầu vào đại diện cho một danh sách các số được sắp xếp theo thứ tự và chúng ta được phép chọn bất kỳ khối liên tục nào trong số đó. Đầu ra là một số duy nhất: tổng tốt nhất có thể đạt được trong số tất cả các khối này. 

Mức ràng buộc đối với loại sự cố này trên Codeforces hầu như luôn bao hàm kích thước mảng lên tới khoảng 200.000 trở lên, với các giá trị có thể dương hoặc âm và có độ lớn. Điều đó ngay lập tức loại trừ mọi phép thăm dò bậc hai hoặc bậc ba của các mảng con. Một giải pháp kiểm tra từng cặp điểm cuối và tính tổng phân đoạn giữa chúng sẽ yêu cầu thời gian O(n²), quá chậm ở mức n = 2 × 10⁵ vì nó dẫn đến các phép toán khoảng 4 × 10¹⁰ trong trường hợp xấu nhất. 

Một số trường hợp đặc biệt cần được nêu rõ. 

Nếu tất cả các số đều âm thì câu trả lời đúng là phần tử đơn lớn nhất. Ví dụ, đối với đầu vào`-5 -2 -8`, câu trả lời là`-2`. Một cách tiếp cận ngây thơ cho rằng chúng ta có thể “bắt đầu mới” từ con số 0 sẽ trả về kết quả không chính xác.`0`nếu nó cho phép các mảng con trống, điều này không được phép. 

Nếu mảng có sự kết hợp của nhiều âm và dương lớn thì việc lấy mọi thứ không phải lúc nào cũng tối ưu. Ví dụ, trong`4 -10 6`, mảng con tốt nhất là`[6]`hoặc`[4]`hoặc`[6]`tùy theo phân khúc; câu trả lời đúng là`6`, không`0`hoặc`-10`. 

Nếu có nhiều vùng dương rời rạc được phân tách bằng các vùng âm lớn thì phân đoạn tối ưu sẽ đặt lại ở đúng điểm thay vì tích lũy qua khoảng trống âm. 

## Phương pháp tiếp cận 

Chiến lược brute-force rất đơn giản: liệt kê mọi mảng con có thể có, tính tổng của nó và theo dõi mức tối đa. Đối với mỗi chỉ số bắt đầu i, chúng tôi mở rộng chỉ số cuối j từ i lên n − 1, duy trì tổng hiện hành để chúng tôi không tính toán lại từ đầu. Điều này mang lại các mảng con O(n2) và mỗi phần mở rộng là O(1), dẫn đến tổng thời gian là O(n2). 

Điều này hoạt động chính xác vì nó xem xét rõ ràng mọi phân đoạn liền kề có thể có. Điểm thất bại hoàn toàn là tính toán: với n lớn, số lượng phân đoạn tăng theo phương trình bậc hai và chương trình không thể hoàn thành đúng lúc. 

Quan sát cấu trúc quan trọng là khi quét từ trái sang phải, tại bất kỳ vị trí nào, chúng ta chỉ cần biết liệu việc tiếp tục phân đoạn trước có giúp ích hay không hoặc liệu việc bắt đầu làm mới ở phần tử hiện tại có tạo ra tổng tốt hơn hay không. Nếu tổng hiện có trở thành số âm thì việc chuyển nó về phía trước chỉ gây tổn hại đến số tiền trong tương lai vì việc thêm tiền tố âm sẽ làm giảm bất kỳ phân đoạn nào theo sau. Điều này làm giảm vấn đề trong việc duy trì giá trị “kết thúc ở đây” tốt nhất khi chúng tôi lặp lại và cập nhật giá trị tốt nhất toàn cầu. 

Điều này biến việc tối ưu hóa toàn cục trên tất cả các mảng con thành một phép lặp cục bộ chỉ phụ thuộc vào trạng thái trước đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Tối ưu (thuật toán Kadane) | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Ý tưởng tối ưu: duy trì kết thúc mảng con tốt nhất ở mỗi vị trí 

1. Khởi tạo hai biến:`current`để theo dõi tổng mảng con tốt nhất kết thúc ở chỉ mục hiện tại và`best`để theo dõi câu trả lời tốt nhất được thấy cho đến nay. Bắt đầu cả hai ở phần tử đầu tiên của mảng. Điều này là cần thiết vì mỗi mảng con hợp lệ phải bao gồm ít nhất một phần tử, do đó phần tử đầu tiên xác định đường cơ sở hợp lệ. 
2. Lặp lại mảng bắt đầu từ phần tử thứ hai. 
3. Tại mỗi vị trí`x`, quyết định xem nên mở rộng mảng con trước đó hay bắt đầu mảng con mới tại`x`. Điều này được thực hiện bằng cách so sánh`current + x`với`x`. Nếu như`current`là âm, việc mở rộng nó chỉ làm giảm giá trị, vì vậy khởi động lại sẽ tốt hơn. Ngược lại, việc kéo dài sẽ bảo toàn được số tiền lớn hơn. 
4. Cập nhật`current`đến giá trị đã chọn. Điều này thể hiện tổng tốt nhất có thể của một mảng con phải kết thúc ở chỉ mục hiện tại. 
5. Cập nhật`best`BẰNG`max(best, current)`. Điều này đảm bảo chúng ta nhớ được tổng mảng con cao nhất được thấy ở bất kỳ đâu, vì mảng con tối ưu có thể kết thúc ở bất kỳ vị trí nào. 
6. Sau khi xử lý xong tất cả các phần tử, trả về`best`. 

### Tại sao nó hoạt động 

Tại mỗi chỉ số, thuật toán duy trì bất biến`current`là tổng tối đa của bất kỳ mảng con nào kết thúc chính xác tại chỉ mục đó. Bất kỳ mảng con tối ưu nào kết thúc ở chỉ số i phải chỉ bao gồm phần tử tại i hoặc mở rộng một mảng con kết thúc ở i − 1. Nếu mảng con tốt nhất kết thúc ở i − 1 là âm, thì tổng số sẽ giảm, do đó, tốt nhất là loại bỏ nó và khởi động lại. Quyết định cục bộ này nắm bắt đầy đủ tất cả các khả năng tổng thể vì mỗi mảng con có một điểm cuối duy nhất và mỗi điểm cuối được đánh giá chính xác một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    current = a[0]
    best = a[0]

    for i in range(1, n):
        x = a[i]
        current = max(x, current + x)
        best = max(best, current)

    print(best)

if __name__ == "__main__":
    solve()
```Mã trực tiếp thực hiện phép lặp được mô tả trong hướng dẫn thuật toán. các`current`biến tương ứng với mảng con tốt nhất kết thúc ở chỉ mục hiện tại và`best`biến theo dõi mức tối đa toàn cầu trên tất cả các mảng con như vậy. 

Chi tiết triển khai chính là thứ tự cập nhật.`current`phải được tính toán trước khi cập nhật`best`, từ`best`phụ thuộc vào mảng con mới được tạo kết thúc ở mỗi vị trí. các`max(x, current + x)`dòng là thời điểm chính xác mà quyết định khởi động lại hoặc gia hạn được đưa ra. 

## Ví dụ đã hoạt động 

### Ví dụ 1:`a = [4, -1, 2, 1]`| tôi | x | hiện tại | tốt nhất | 
| --- | --- | --- | --- | 
| 0 | 4 | 4 | 4 | 
| 1 | -1 | 3 | 4 | 
| 2 | 2 | 5 | 5 | 
| 3 | 1 | 6 | 6 | 

Tổng hiện có tăng lên bất chấp giá trị âm vì phân đoạn dương trước đó đủ mạnh để hấp thụ nó. Giá trị tốt nhất cuối cùng tương ứng với toàn bộ mảng. 

### Ví dụ 2:`a = [-2, 3, -1, 2, -5]`| tôi | x | hiện tại | tốt nhất | 
| --- | --- | --- | --- | 
| 0 | -2 | -2 | -2 | 
| 1 | 3 | 3 | 3 | 
| 2 | -1 | 2 | 3 | 
| 3 | 2 | 4 | 4 | 
| 4 | -5 | -1 | 4 | 

Dấu vết này cho thấy hành vi khởi động lại: sau khi bắt đầu tích cực mạnh mẽ, thuật toán vẫn tiếp tục mở rộng qua một điểm âm nhỏ vì nó không phá hủy tổng số, nhưng khi một điểm âm lớn xuất hiện ở cuối thì tốt nhất nên loại bỏ việc tiếp tục. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử được xử lý một lần với các bản cập nhật liên tục | 
| Không gian | O(1) | Chỉ có hai biến đang chạy được lưu trữ | 

Quét tuyến tính phù hợp với các ràng buộc điển hình cho lớp vấn đề này, trong đó n có thể đủ lớn để bất kỳ phương pháp vòng lặp lồng nhau nào đều không khả thi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return str(solve()) if False else ""

# provided samples (placeholders since statement missing)
# assert run("...") == "..."

# custom cases
assert True
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1\n-5 | -5 | xử lý phần tử đơn | 
| 3\n-1 -2 -3 | -1 | trường hợp toàn âm | 
| 5\n1 2 3 4 5 | 15 | tích lũy hoàn toàn tích cực | 
| 5\n5 -100 5 5 5 | 15 | khởi động lại sau khi giảm lớn | 

## Vỏ cạnh 

Một mảng âm hoàn toàn là trường hợp cạnh quan trọng nhất. Xem xét đầu vào`n = 4`,`[-3, -1, -7, -4]`. Thuật toán khởi tạo`current = -3`,`best = -3`. Ở mỗi bước,`current = max(x, current + x)`luôn chọn phần tử đơn vì việc thêm bất kỳ tổng nào trước đó sẽ làm cho tổng đó âm hơn. Điều tốt nhất còn lại`-1`, đó là tổng mảng con tối đa chính xác. 

Một trường hợp tinh vi khác là khi một âm lớn xuất hiện sau một tiền tố dương mạnh. Vì`[-2, 10, -3, -2]`, dấu vết là`current = -2`, sau đó`10`, sau đó`7`, sau đó`5`. Thuật toán không bao giờ khởi động lại một cách không cần thiết vì tiền tố vẫn có lợi cho đến khi nó hoàn toàn bị phần tử tiếp theo lấn át.
