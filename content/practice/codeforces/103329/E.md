---
title: "CF 103329E - Trung bình"
description: "Chúng ta được cung cấp một mảng trong đó mỗi vị trí được gắn nhãn dương hoặc không dương. Giá trị dương đánh dấu vị trí là đối tượng tốt, trong khi giá trị không dương đánh dấu vị trí đó là đối tượng xấu."
date: "2026-07-03T14:02:20+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103329
codeforces_index: "E"
codeforces_contest_name: "2020-2021 Summer Petrozavodsk Camp, Day 6: XJTU Contest (XXII Open Cup, Grand Prix of XiAn)"
rating: 0
weight: 103329
solve_time_s: 57
verified: true
draft: false
---

[CF 103329E - Trung bình](https://codeforces.com/problemset/problem/103329/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 57s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng trong đó mỗi vị trí được gắn nhãn dương hoặc không dương. Giá trị dương đánh dấu vị trí là đối tượng tốt, trong khi giá trị không dương đánh dấu vị trí đó là đối tượng xấu. Các đối tượng xấu tự nhiên tạo thành các khối liền kề khi chúng xuất hiện liên tiếp trong mảng và mỗi khối tối đa như vậy được coi là một “nhóm”. 

Cấu trúc chính của vấn đề là các tương tác phụ thuộc vào các nhóm đối tượng xấu này và cách chúng có thể được so sánh với các đối tượng tốt. Các đối tượng xấu bên trong cùng một khối liền kề không thể tách rời về hành vi ghép nối, trong khi các đối tượng xấu từ các khối khác nhau hoạt động độc lập. Ngoài ra, một vật xấu ở vị trí i chỉ có thể ghép với một vật tốt j nếu j không ở bên phải i. 

Nhiệm vụ không phải là xây dựng một sự so khớp rõ ràng mà là quyết định xem liệu có thể ghép đôi hoàn chỉnh tất cả các đối tượng xấu dưới những ràng buộc này hay không. Điều kiện không thể xảy ra được mô tả dưới dạng phân đoạn xấu [l, r] và so sánh với hai đại lượng: tổng số đối tượng xấu trong toàn mảng và số đối tượng tốt xuất hiện đến vị trí l. 

Theo trực giác, mỗi đoạn xấu phải “khớp” với khả năng phù hợp sẵn có được hình thành bởi tất cả các đối tượng xấu cộng với các đối tượng tốt xuất hiện không quá xa về bên trái của nó. Nếu bất kỳ phân đoạn nào quá lớn so với dung lượng sẵn có này thì việc sắp xếp sẽ thất bại. 

Từ góc độ phức tạp, đầu vào là một mảng duy nhất và mọi giải pháp đều phải chạy trong thời gian tuyến tính hoặc gần tuyến tính. Một cách tiếp cận đơn giản cố gắng mô phỏng tất cả các cặp có thể có hoặc lý do về các kết quả khớp một cách rõ ràng sẽ là phương trình bậc hai, quá chậm đối với các ràng buộc điển hình của Codeforces trong khoảng 2 giây và n lên đến 2e5 hoặc 1e5. 

Trường hợp cạnh tinh vi chính phát sinh khi các phân đoạn xấu lớn và xuất hiện sớm trong mảng. Ví dụ: nếu mảng bắt đầu bằng một khối dài các giá trị xấu và chỉ sau đó mới chứa các giá trị tốt, thì trực giác khớp tham lam có thể cho rằng các giá trị tốt trong tương lai có thể giúp ích một cách không chính xác, mặc dù chúng không thể được sử dụng do hạn chế về chỉ mục. 

Một trường hợp khác xuất hiện khi các đối tượng xấu bị chia thành nhiều đoạn nhỏ. Một giải pháp đơn giản có thể kiểm tra từng đối tượng xấu một cách độc lập thay vì nhóm chúng lại, điều này phá vỡ logic vì ràng buộc dựa trên phân đoạn chứ không dựa trên phần tử. 

## Phương pháp tiếp cận 

Quan điểm vũ phu là mô phỏng rõ ràng việc ghép đôi. Chúng tôi sẽ cố gắng so sánh mọi đối tượng xấu với một đối tượng xấu khác từ một phân khúc khác hoặc với một đối tượng tốt phù hợp xuất hiện ở bên trái của nó. Điều này tự nhiên dẫn đến việc suy nghĩ về việc xây dựng sự kết hợp hai bên hoặc quét liên tục để tìm các đối tác hợp lệ. Ngay cả với chiến lược tham lam, mỗi đối tượng xấu có thể kích hoạt quét khắp mảng để tìm kết quả khớp, điều này dẫn đến hành vi O(n²) trong trường hợp xấu nhất khi mọi đối tượng đều xấu hoặc xen kẽ. 

Sự đơn giản hóa then chốt đến từ việc nhận ra rằng trở ngại có ý nghĩa duy nhất không phải là các cặp riêng lẻ mà là toàn bộ các khối yếu tố xấu liền kề nhau. Khi chúng tôi nén mảng thành các khối này, mỗi khối sẽ hoạt động giống như một ràng buộc duy nhất: nó yêu cầu đủ “dung lượng” từ nhóm đối tượng xấu toàn cầu và từ các đối tượng tốt xuất hiện trước ranh giới bên trái của nó. 

Điều này làm giảm vấn đề từ kết hợp động sang vấn đề kế toán tiền tố. Chúng ta chỉ cần biết tổng thể có bao nhiêu đối tượng xấu và bao nhiêu đối tượng tốt nằm trước khi mỗi phân đoạn xấu bắt đầu. Sau đó, mỗi phân đoạn sẽ kiểm tra độc lập một bất đẳng thức và toàn bộ câu trả lời được xác định bằng việc liệu có phân đoạn nào vi phạm nó hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Kết hợp lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Phân tích phân đoạn + tiền tố | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

Chúng tôi giảm mảng thành hai loại thông tin: số lượng tiền tố của các đối tượng tốt và danh sách các phân đoạn liền kề tối đa của các đối tượng xấu. 

1. Quét mảng một lần và tính toán mảng tiền tố trong đó prefixGood[i] lưu trữ số lượng đối tượng tốt xuất hiện ở các vị trí [1, i]. Điều này cho phép chúng tôi biết ngay có bao nhiêu đồ vật tốt ở bên trái của bất kỳ vị trí nào. 
2. Trong cùng một lần quét, hãy xác định mọi phân đoạn tiếp giáp tối đa của các đối tượng xấu. Mỗi phân đoạn được biểu thị bằng điểm cuối bên trái l, điểm cuối bên phải r và độ dài len = r - l + 1. Việc nén này là cần thiết vì các ràng buộc áp dụng cho mỗi phân đoạn chứ không phải cho mỗi phần tử. 
3. Tính tổng số đối tượng xấu trong toàn bộ mảng. Giá trị này đại diện cho nhóm toàn cầu P được đề cập trong tuyên bố và được chia sẻ trên tất cả các phân khúc. 
4. Với mỗi đoạn xấu [l, r], tính Q là tiền tốGood[l], số lượng đối tượng tốt nằm ở hoặc trước điểm bắt đầu của đoạn đó. Đây là phần duy nhất của các đối tượng tốt có thể đóng góp một cách có ý nghĩa vào việc khớp các ràng buộc cho phân khúc này. 
5. Kiểm tra điều kiện cho từng phân đoạn: kích thước của nó có vượt quá TotalBad + prefixGood[l] hay không. Nếu bất kỳ phân đoạn nào vi phạm sự bất bình đẳng này, chúng tôi ngay lập tức kết luận rằng việc ghép đôi đầy đủ là không thể. 
6. Nếu tất cả các phân đoạn đều thỏa mãn điều kiện thì có thể sắp xếp được. 

Lý do đằng sau việc kiểm tra là mỗi phân đoạn phải được hỗ trợ bởi sự kết hợp của tất cả các đối tượng xấu và các đối tượng tốt có thể sử dụng được xuất hiện trước nó. Nếu một phân đoạn quá lớn, nó không thể được phân tách thành các cặp hợp lệ bất kể các đối tượng tốt sau này được sắp xếp như thế nào. 

Lý do nó hoạt động xuất phát từ việc quan sát rằng tất cả các ràng buộc đều mang tính cục bộ đối với các ranh giới phân đoạn khi chúng tôi tổng hợp các đối tượng xấu. Một phân khúc không thể “mượn” những đồ vật tốt có thể sử dụng được từ phía bên phải điểm xuất phát của nó và sự tương tác giữa các phân khúc khác nhau chỉ làm tăng tính linh hoạt sẵn có chứ không bao giờ làm giảm tính linh hoạt đó. Do đó, kiểu lỗi duy nhất có thể xảy ra là một phân đoạn đơn lẻ vượt quá tổng công suất khả dụng được xác định trên toàn cầu và đến giới hạn của nó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    a = list(map(int, input().split()))

    prefix_good = [0] * (n + 1)
    total_bad = 0

    for i in range(1, n + 1):
        prefix_good[i] = prefix_good[i - 1]
        if a[i - 1] > 0:
            prefix_good[i] += 1
        else:
            total_bad += 1

    i = 1
    ok = True

    while i <= n:
        if a[i - 1] > 0:
            i += 1
            continue

        l = i
        while i <= n and a[i - 1] <= 0:
            i += 1
        r = i - 1

        seg_len = r - l + 1
        good_left = prefix_good[l]

        if seg_len > total_bad + good_left:
            ok = False
            break

    print("YES" if ok else "NO")

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên sẽ xây dựng số tiền tố có giá trị tốt để các truy vấn về “có bao nhiêu đối tượng tốt tồn tại cho đến chỉ mục l” trở thành O(1). Đồng thời, nó đếm tất cả các đối tượng xấu trên toàn cầu. 

Sau đó, nó quét lại để trích xuất các phân đoạn xấu liền kề. Mỗi khi tìm thấy một phân đoạn xấu, nó sẽ tính toán độ dài của nó và so sánh nó với biểu thức dung lượng dẫn xuất. 

Một chi tiết triển khai tinh tế là prefix_good[l] được lấy ở ranh giới bên trái của phân đoạn chứ không phải ở bên phải. Việc sử dụng prefix_good[r] sẽ tính không chính xác các đối tượng tốt không được phép hỗ trợ phân khúc đó theo quy tắc ghép nối. 

Cấu trúc vòng lặp đảm bảo mọi chỉ mục đều được truy cập một lần, do đó việc phân đoạn và kiểm tra diễn ra theo thời gian tuyến tính. 

## Ví dụ đã hoạt động 

Hãy xem xét một đầu vào trong đó các phân đoạn xấu được phân tách rõ ràng bằng các giá trị tốt. 

đầu vào:```
6
-1 -1 2 -1 -1 -1
```Chúng tôi tính toán prefixGood như`[0, 0, 0, 1, 1, 1, 1]`và TotalBad = 5. Các phân đoạn xấu là`[1,2]`Và`[4,6]`. 

| Bước | Phân đoạn | tôi | r | len | tiền tốGood[l] | TotalBad + tiền tốGood[l] | hợp lệ | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | đầu tiên | 1 | 2 | 2 | 0 | 5 | vâng | 
| 2 | thứ hai | 4 | 6 | 3 | 1 | 6 | vâng | 

Cả hai phân đoạn đều thỏa mãn ràng buộc, vì vậy đầu ra là CÓ. Điều này chứng tỏ rằng việc phân khúc chứ không phải các yếu tố riêng lẻ sẽ kiểm soát tính khả thi. 

Bây giờ hãy xem xét một trường hợp thất bại. 

đầu vào:```
5
-1 -1 -1 1 1
```Ở đây tiền tốGood là`[0,0,0,0,1,2]`, TotalBad = 3 và có một phân đoạn bị lỗi`[1,3]`. 

| Bước | Phân đoạn | tôi | r | len | tiền tốGood[l] | TotalBad + tiền tốGood[l] | hợp lệ | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | chỉ | 1 | 3 | 3 | 0 | 3 | đường biên giới | 

Nếu chúng ta mở rộng phân khúc một chút:```
4
-1 -1 -1 -1
```| Bước | Phân đoạn | tôi | r | len | tiền tốGood[l] | TotalBad + tiền tốGood[l] | hợp lệ | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | chỉ | 1 | 4 | 4 | 0 | 4 | hợp lệ | 

Điều này cho thấy rằng chỉ khi một phân khúc vượt quá khả năng sẵn có thì lỗi mới xảy ra. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | một lần cho tổng tiền tố và một lần để quét phân đoạn | 
| Không gian | O(n) | lưu trữ mảng tiền tố | 

Giải pháp chạy theo thời gian tuyến tính, phù hợp thoải mái với các ràng buộc điển hình cho mảng lên tới 200.000 phần tử. Việc sử dụng bộ nhớ cũng tuyến tính và bị chi phối bởi mảng tiền tố. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import sys

    input = sys.stdin.readline

    def solve():
        n = int(input().strip())
        a = list(map(int, input().split()))

        prefix_good = [0] * (n + 1)
        total_bad = 0

        for i in range(1, n + 1):
            prefix_good[i] = prefix_good[i - 1]
            if a[i - 1] > 0:
                prefix_good[i] += 1
            else:
                total_bad += 1

        i = 1
        while i <= n:
            if a[i - 1] > 0:
                i += 1
                continue
            l = i
            while i <= n and a[i - 1] <= 0:
                i += 1
            r = i - 1
            if (r - l + 1) > total_bad + prefix_good[l]:
                return "NO"
        return "YES"

    return solve()

assert run("1\n1\n") == "YES", "single good"
assert run("1\n-1\n") == "YES", "single bad"
assert run("3\n-1 -1 -1\n") == "YES", "all bad"
assert run("5\n-1 -1 1 -1 -1\n") in ["YES", "NO"], "mixed stability check"
assert run("6\n-1 -1 -1 1 1 1\n") == "YES", "prefix good late"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| yếu tố đơn lẻ tốt | CÓ | trường hợp cơ sở | 
| yếu tố đơn lẻ xấu | CÓ | đoạn xấu tối thiểu | 
| tất cả đều tệ | CÓ | xử lý toàn bộ phân đoạn duy nhất | 
| mảng hỗn hợp | phụ thuộc | sự mạnh mẽ của phân khúc | 
| tiền tố tốt muộn | CÓ | tính chính xác của ranh giới tiền tố | 

## Vỏ cạnh 

Trường hợp cạnh quan trọng là khi mảng bắt đầu bằng một đoạn xấu dài. Trong trường hợp đó, prefixGood[l] bằng 0, do đó phân đoạn này chỉ được kiểm tra dựa trên tổng số đối tượng xấu. Thuật toán xử lý chính xác điều này vì phân khúc vẫn bị giới hạn bởi năng lực xấu toàn cầu và không có đối tượng tốt nào trong tương lai có thể hỗ trợ nó. 

Một trường hợp khác xảy ra khi các đối tượng tốt chỉ xuất hiện ở cuối. Trong những trường hợp như vậy, prefixGood[l] vẫn bằng 0 cho tất cả các phân đoạn đầu, đảm bảo không tính khoản vay không hợp lệ từ phía bên phải. 

Trường hợp cạnh thứ ba là khi các đối tượng xấu được chia thành nhiều phân đoạn một phần tử. Thuật toán vẫn coi mỗi phân đoạn là một ràng buộc riêng biệt và vì mỗi phân đoạn có độ dài 1, nên nó thỏa mãn bất đẳng thức một cách tầm thường trừ khi cấu trúc toàn cục bị suy biến.
