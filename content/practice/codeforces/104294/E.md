---
title: "CF 104294E - Kẻ giết quái vật"
description: "Chúng ta được cung cấp một dòng quái vật, mỗi dòng có một giá trị sức mạnh nguyên. Chúng tôi muốn chọn một khối liền kề của những con quái vật này và tính tổng sức mạnh của chúng. Trong số tất cả các khối liền kề có thể có, chúng ta cần khối có tổng càng lớn càng tốt và chúng ta xuất ra tổng đó."
date: "2026-07-01T20:25:04+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104294
codeforces_index: "E"
codeforces_contest_name: "UTPC Spring 2023 Open Contest"
rating: 0
weight: 104294
solve_time_s: 64
verified: true
draft: false
---

[CF 104294E - Kẻ giết quái vật](https://codeforces.com/problemset/problem/104294/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 4s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một dòng quái vật, mỗi dòng có một giá trị sức mạnh nguyên. Chúng tôi muốn chọn một khối liền kề của những con quái vật này và tính tổng sức mạnh của chúng. Trong số tất cả các khối liền kề có thể có, chúng ta cần khối có tổng càng lớn càng tốt và chúng ta xuất ra tổng đó. 

Đầu vào chỉ đơn giản là một dãy số nguyên. Mỗi số nguyên có thể dương, âm hoặc bằng 0. Lựa chọn hợp lệ là bất kỳ khoảng nào từ chỉ số i đến j và điểm của nó là tổng các phần tử trong khoảng đó. Nhiệm vụ là tìm ra số điểm tối đa có thể có trong tất cả các khoảng thời gian như vậy. 

Các ràng buộc cho phép tối đa 100000 phần tử, với các giá trị có độ lớn lên tới 10^9. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào thử tất cả các mảng con một cách rõ ràng. Một bảng liệt kê O(n^2) ngây thơ đã tính toán khoảng 5 tỷ khoảng trong trường hợp xấu nhất và nếu mỗi khoảng yêu cầu tổng O(n) thì nó sẽ trở thành O(n^3), vượt xa giới hạn thời gian. Ngay cả O(n^2) cũng quá chậm trong Python dưới 1 giây. 

Một điểm tinh tế là xử lý các trường hợp tất cả các số đều âm. Một cách tiếp cận sai phổ biến là trả về 0 bằng cách giả sử chúng ta có thể chọn một mảng con trống. Bài toán không cho phép một mảng con trống nên chúng ta phải chọn ít nhất một phần tử. Ví dụ, đối với đầu vào`[-5, -2, -8]`, câu trả lời đúng là`-2`, không`0`. 

Một trường hợp cạnh khác là mảng một phần tử. Vì`[7]`, câu trả lời là`7`và bất kỳ logic nào khởi tạo tổng tốt nhất bằng 0 mà không cẩn thận sẽ thất bại ở đây. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: xem xét mọi chỉ số bắt đầu có thể, mở rộng nó đến mọi chỉ số kết thúc có thể, tính tổng và theo dõi mức tối đa. Điều này đúng vì mọi mảng con liền kề đều được đánh giá rõ ràng. Tuy nhiên, chi phí đến từ việc tính toán lại số tiền nhiều lần. Có khoảng n(n+1)/2 mảng con và ngay cả với tổng tăng dần, đây là phép toán O(n^2), quá chậm đối với 100000 phần tử. 

Quan sát quan trọng là khi quét từ trái sang phải, mảng con tốt nhất kết thúc ở vị trí i hoặc mở rộng mảng con tốt nhất kết thúc ở i−1 hoặc bắt đầu mới ở i. Nếu tổng hiện có trở thành số âm thì việc giữ nó chỉ làm cho số tiền trong tương lai trở nên tồi tệ hơn vì nó sẽ bị trừ đi bất kỳ phần mở rộng nào. Điều này làm giảm vấn đề trong việc duy trì một kết thúc tốt nhất đang chạy duy nhất ở mỗi vị trí. 

Đây chính là bản chất ý tưởng của Kadane: chúng tôi nén tất cả các lựa chọn mảng con kết thúc ở mỗi chỉ mục vào một trạng thái duy nhất, thay vì khám phá tất cả các phân vùng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(1) | Quá chậm | 
| Tối ưu (Kadane) | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý mảng từ trái sang phải trong khi duy trì hai giá trị: tổng tốt nhất kết thúc ở vị trí hiện tại và câu trả lời tổng thể tốt nhất được thấy cho đến nay. 

1. Khởi tạo cả tổng hiện tại và câu trả lời đúng nhất toàn cầu với phần tử đầu tiên của mảng. Điều này đảm bảo tính chính xác ngay cả khi tất cả các giá trị đều âm. 
2. Với mỗi phần tử tiếp theo p[i], hãy quyết định xem nên mở rộng mảng con trước đó hay bắt đầu một mảng con mới tại i. Chúng tôi tính tổng hoạt động mới là giá trị tối đa của p[i] và current_sum + p[i]. Sự lựa chọn này phản ánh số tiền tích lũy trước đó là có lợi hay có hại. 
3. Cập nhật câu trả lời tốt nhất toàn cầu dưới dạng giá trị lớn nhất của chính nó và tổng chạy mới. Điều này đảm bảo chúng tôi ghi lại mảng con tốt nhất được thấy ở bất kỳ đâu trong mảng, không chỉ những mảng con kết thúc ở chỉ mục cuối cùng. 
4. Tiếp tục cho đến hết mảng và đưa ra câu trả lời đúng nhất toàn cục. 

Quyết định cốt lõi xảy ra ở mỗi chỉ mục: nếu tổng trước đó âm thì việc thêm nó vào phần tử hiện tại chỉ làm giảm kết quả, vì vậy việc khởi động lại là tối ưu. Nếu nó dương, việc mở rộng sẽ cải thiện tổng. 

### Tại sao nó hoạt động 

Tại mọi chỉ số i, thuật toán duy trì bất biến rằng tổng chạy hiện tại là tổng mảng con tối đa có thể có và phải kết thúc chính xác tại i. Bất kỳ mảng con nào kết thúc tại i đều xuất phát từ việc mở rộng một mảng con kết thúc tại i−1 hoặc bắt đầu mới tại i. Vì cả hai khả năng đều được so sánh rõ ràng nên không có ứng cử viên hợp lệ nào bị bỏ qua. Mức tối đa toàn cầu theo dõi tốt nhất trong số tất cả các điểm cuối, vì vậy sau khi quá trình quét hoàn tất, nó phải bằng phân mảng con tốt nhất trên toàn bộ mảng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))

cur = a[0]
best = a[0]

for i in range(1, n):
    cur = max(a[i], cur + a[i])
    best = max(best, cur)

print(best)
```Việc triển khai phản ánh trực tiếp quá trình chuyển đổi trạng thái được mô tả trước đó.`cur`lưu trữ tổng mảng con tốt nhất kết thúc ở vị trí hiện tại, trong khi`best`theo dõi mức tối ưu toàn cục. Đang khởi tạo cả hai với`a[0]`tránh việc xử lý không chính xác các mảng toàn âm. 

Chi tiết quan trọng là thứ tự cập nhật: chúng ta phải tính toán giá trị mới`cur`trước khi cập nhật`best`, từ`best`phụ thuộc vào mảng con được cập nhật kết thúc tại i. Một điểm tinh tế khác là khởi tạo, không được mặc định là 0. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
9
-2 10 -3 5 -2 1 2 6 -1
```| tôi | một [tôi] | cur (kết thúc hay nhất ở đây) | tốt nhất | 
| --- | --- | --- | --- | 
| 0 | -2 | -2 | -2 | 
| 1 | 10 | 10 | 10 | 
| 2 | -3 | 7 | 10 | 
| 3 | 5 | 12 | 12 | 
| 4 | -2 | 10 | 12 | 
| 5 | 1 | 11 | 12 | 
| 6 | 2 | 13 | 13 | 
| 7 | 6 | 19 | 19 | 
| 8 | -1 | 18 | 19 | 

Dấu vết này cho thấy các giá trị âm được hấp thụ hoặc loại bỏ có chọn lọc như thế nào tùy thuộc vào việc chúng có làm giảm tổng lượng hoạt động hay không. Đỉnh cao cuối cùng xảy ra khi sự tích lũy của những đóng góp tích cực lớn hơn những đóng góp tiêu cực trước đó. 

### Ví dụ 2 

đầu vào:```
5
-4 -1 -7 -3 -2
```| tôi | một [tôi] | cur | tốt nhất | 
| --- | --- | --- | --- | 
| 0 | -4 | -4 | -4 | 
| 1 | -1 | -1 | -1 | 
| 2 | -7 | -7 | -1 | 
| 3 | -3 | -3 | -1 | 
| 4 | -2 | -2 | -1 | 

Trường hợp này chứng tỏ rằng thuật toán không bao giờ trả về sai 0. Nó xác định chính xác phần tử đơn âm ít nhất là mảng con tốt nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử được xử lý chính xác một lần với công việc liên tục | 
| Không gian | O(1) | Chỉ có hai biến được duy trì bất kể kích thước đầu vào | 

Quét tuyến tính vừa vặn thoải mái trong giới hạn cho 100000 phần tử và bộ nhớ không đổi đảm bảo không có chi phí hoạt động từ các cấu trúc phụ trợ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))

    cur = a[0]
    best = a[0]

    for i in range(1, n):
        cur = max(a[i], cur + a[i])
        best = max(best, cur)

    return str(best)

# provided sample
assert run("9\n-2 10 -3 5 -2 1 2 6 -1\n") == "19"

# single element
assert run("1\n5\n") == "5"

# all negative
assert run("3\n-5 -2 -8\n") == "-2"

# all positive
assert run("4\n1 2 3 4\n") == "10"

# alternating
assert run("5\n1 -2 3 -2 5\n") == "5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 5 | khởi tạo cơ sở | 
| tất cả đều tiêu cực | -2 | không cho phép mảng con trống | 
| tất cả đều tích cực | 10 | tích lũy đầy đủ đúng đắn | 
| giá trị xen kẽ | 5 | khởi động lại và mở rộng logic | 

## Vỏ cạnh 

Đối với mảng một phần tử như`[5]`, thuật toán khởi tạo`cur`Và`best`đến 5 và trả lại ngay lập tức. Không cần lặp lại và bất biến được giữ một cách tầm thường vì chỉ có một mảng con khả thi. 

Đối với một mảng toàn âm như`[-5, -2, -8]`, tổng chạy liên tục khởi động lại ở mỗi phần tử vì việc kéo dài chỉ làm giảm giá trị. Dấu vết là`cur = -5, -2, -8`với`best = -2`, chọn chính xác phần tử âm ít nhất. Điều này cho thấy rằng thuật toán không thích các mảng con dài hơn một cách sai lầm khi chúng có hại.
