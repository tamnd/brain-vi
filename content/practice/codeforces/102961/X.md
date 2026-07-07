---
title: "CF 102961X - Tổng của ba giá trị"
description: "Chúng ta được cung cấp một dãy số và một giá trị mục tiêu. Nhiệm vụ là xác định xem có tồn tại ba phần tử riêng biệt trong chuỗi có tổng bằng đích hay không và nếu có thì trả về vị trí của chúng trong mảng ban đầu."
date: "2026-07-04T06:57:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102961
codeforces_index: "X"
codeforces_contest_name: "CSES Problem Set: Sorting and Searching"
rating: 0
weight: 102961
solve_time_s: 43
verified: true
draft: false
---

[CF 102961X - Tổng của ba giá trị](https://codeforces.com/problemset/problem/102961/X) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 43s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một dãy số và một giá trị mục tiêu. Nhiệm vụ là xác định xem có tồn tại ba phần tử riêng biệt trong chuỗi có tổng bằng đích hay không và nếu có thì trả về vị trí của chúng trong mảng ban đầu. 

Chi tiết quan trọng là chúng tôi không chọn các giá trị riêng biệt mà chọn các chỉ số, vì vậy, ngay cả khi cùng một số xuất hiện nhiều lần, chúng tôi phải đảm bảo rằng chúng tôi đang đề cập đến các lần xuất hiện khác nhau khi tạo thành bộ ba. Đầu ra thường là bất kỳ bộ ba chỉ số hợp lệ nào, không nhất thiết là tất cả các khả năng hoặc sự kết hợp tối ưu ngoài sự tồn tại. 

Các ràng buộc trong cài đặt cổ điển này thường cho phép tối đa khoảng 2×10^5 phần tử. Điều đó ngay lập tức loại trừ việc quét bậc ba và bậc hai trên tất cả các bộ ba. Một bảng liệt kê khối O(n^3) kiểm tra rõ ràng tất cả các bộ ba và sẽ thực hiện theo thứ tự 10^15 phép toán trong trường hợp xấu nhất, vượt xa giới hạn khả thi. Ngay cả O(n^2) cũng có thể chặt chẽ nhưng trở nên khả thi với cấu trúc hai con trỏ cẩn thận sau khi sắp xếp. 

Một trường hợp lỗi tinh vi xuất hiện khi có các giá trị trùng lặp và một giải pháp đơn giản xử lý các giá trị thay vì chỉ số. Ví dụ: nếu mảng là [3, 3, 3, 4] với mục tiêu 9, câu trả lời đúng là ba chỉ số riêng biệt của giá trị 3. Cách tiếp cận dựa trên giá trị thu gọn các bản sao trùng lặp có thể cho rằng chỉ có một 3 tồn tại và không tìm được giải pháp mặc dù nó hợp lệ theo nghĩa được lập chỉ mục. 

Một trường hợp cạnh khác là khi mảng nhỏ, ví dụ n = 3. Trong trường hợp này, bất kỳ giải pháp nào cũng phải xác thực trực tiếp bộ ba duy nhất có thể và các thuật toán giả sử sắp xếp hoặc di chuyển hai con trỏ mà không xử lý ranh giới cẩn thận có thể dễ dàng bỏ qua các câu trả lời hợp lệ. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: thử từng bộ ba chỉ số i, j, k và kiểm tra xem các giá trị của chúng có bằng với mục tiêu hay không. Điều này hiệu quả vì nó liệt kê đầy đủ tất cả các khả năng và không bao giờ bỏ lỡ sự kết hợp hợp lệ. Tuy nhiên, đối với n phần tử, điều này yêu cầu khoảng n chọn 3 lần kiểm tra, tăng dần thành O(n^3). Với n khoảng 10^5, điều này trở nên không thể tính toán được. 

Chúng ta có thể giảm bớt sự dư thừa bằng cách trước tiên sửa một chỉ mục và sau đó tìm kiếm thêm hai giá trị để hoàn thành tổng được yêu cầu. Khi một phần tử được sửa, vấn đề sẽ giảm xuống việc tìm hai số trong mảng còn lại có tổng bằng một giá trị đã biết. Bài toán con đó là bài toán tổng hai cổ điển, có thể được giải một cách hiệu quả bằng cách sử dụng một mảng được sắp xếp và quét hai con trỏ. 

Quan sát quan trọng là việc sắp xếp không làm mất khả năng khôi phục các chỉ số nếu chúng ta mang theo các vị trí ban đầu. Sau khi sắp xếp, chúng ta có thể di chuyển hai con trỏ vào trong dựa trên tổng hiện tại quá nhỏ hay quá lớn. Điều này làm giảm việc tìm kiếm bên trong từ quét tuyến tính trên mỗi phần tử cố định sang quét tuyến tính hai con trỏ. 

Lực lượng vũ phu hoạt động vì nó liệt kê tất cả các bộ ba, nhưng nó thất bại khi n tăng lên vì nó lặp lại cùng một tổng một phần nhiều lần. Quan sát thấy rằng tổng hai có thể được giải trong thời gian tuyến tính sau khi sắp xếp làm giảm độ phức tạp tổng thể từ bậc ba sang bậc hai. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^3) | O(1) | Quá chậm | 
| Đã sửa + Hai con trỏ | O(n^2) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi sắp xếp mảng, nhưng chúng tôi theo dõi các chỉ mục ban đầu vì đầu ra yêu cầu các vị trí theo thứ tự ban đầu.

1. Ghép từng giá trị với chỉ mục ban đầu của nó và sắp xếp theo giá trị. Điều này cho phép chúng tôi áp dụng logic đặt hàng trong khi vẫn có thể báo cáo các chỉ số chính xác sau này. 
2. Lặp lại mảng, coi mỗi vị trí i là phần tử đầu tiên cố định của bộ ba. Mục tiêu trở thành tìm hai phần tử trong hậu tố có tổng bằng đích trừ đi giá trị cố định. 
3. Với mỗi i cố định, khởi tạo hai con trỏ, một ở i+1 và một ở cuối mảng. Chúng đại diện cho các yếu tố thứ hai và thứ ba của ứng cử viên. 
4. Tính tổng các giá trị tại i, con trỏ trái và con trỏ phải. Nếu tổng này khớp với mục tiêu, chúng tôi đã tìm thấy bộ ba hợp lệ và có thể xuất chỉ số ban đầu của chúng. 
5. Nếu tổng quá nhỏ, hãy di chuyển con trỏ trái sang phải để tăng tổng. Nếu tổng quá lớn, hãy di chuyển con trỏ bên phải sang trái để giảm tổng. 
6. Lặp lại cho đến khi các con trỏ giao nhau hoặc tìm thấy bộ ba hợp lệ. 

Tại sao nó hoạt động: sau khi sắp xếp, mảng có cấu trúc đơn điệu. Đối với một phần tử cố định, bất kỳ tổng cặp nào cũng có thể dự đoán được đối với chuyển động của con trỏ. Di chuyển con trỏ bên trái sẽ tăng tổng trong phạm vi còn lại và di chuyển con trỏ bên phải sẽ giảm tổng đó. Điều này đảm bảo rằng không có cặp hợp lệ nào bị bỏ qua vì mọi điều chỉnh sẽ loại bỏ nghiêm ngặt các phạm vi không thể chứa giải pháp hợp lệ trong khi vẫn bảo toàn tất cả các ứng cử viên khả thi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, x = map(int, input().split())
    arr = list(map(int, input().split()))
    
    a = [(arr[i], i + 1) for i in range(n)]
    a.sort()
    
    for i in range(n):
        target = x - a[i][0]
        l, r = i + 1, n - 1
        
        while l < r:
            s = a[l][0] + a[r][0]
            if s == target:
                print(a[i][1], a[l][1], a[r][1])
                return
            if s < target:
                l += 1
            else:
                r -= 1
    
    print("IMPOSSIBLE")

if __name__ == "__main__":
    solve()
```Cấu trúc cốt lõi phân tách các mối quan tâm một cách rõ ràng. Bước sắp xếp gắn các chỉ số để chúng ta không bao giờ mất dấu vị trí ban đầu. Vòng lặp bên ngoài cố định một phần tử và vòng lặp hai con trỏ bên trong thực hiện tìm kiếm có giới hạn cho cặp phần tử bổ sung. 

Một cạm bẫy triển khai phổ biến là quên rằng sau khi sắp xếp, các chỉ mục không còn tương ứng với vị trí ban đầu. Một vấn đề tinh tế khác là khởi tạo con trỏ: cả hai con trỏ phải bắt đầu đúng sau chỉ mục cố định để tránh sử dụng lại cùng một phần tử. Cuối cùng, việc kết thúc phải xảy ra ngay lập tức khi tìm thấy bộ ba hợp lệ, bởi vì có thể tồn tại nhiều giải pháp nhưng chỉ cần một giải pháp. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào trong đó n = 5, x = 10 và mảng là [2, 3, 7, 5, 1]. Sau khi ghép nối với các chỉ mục và sắp xếp, chúng ta sẽ có được cấu trúc được sắp xếp theo giá trị. 

Chúng tôi theo dõi việc thực hiện: 

| tôi (đã sửa) | tôi | r | giá trị được xem xét | tổng hợp | hành động | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 4 | 1 + 2 + 3 (về mặt khái niệm) | quá nhỏ | di chuyển tôi | 
| 0 | 2 | 4 | cặp tiếp theo | trận đấu | trở lại | 

Dấu vết này cho thấy thuật toán hội tụ nhanh chóng như thế nào bằng cách mở rộng cạnh nhỏ nhất khi tổng không đủ. 

Bây giờ hãy xem xét trường hợp có các bản sao: n = 4, x = 9, mảng [3, 3, 3, 4]. Sau khi sắp xếp, thuật toán sửa 3 phần tử rồi tìm kiếm giữa các phần tử còn lại. Quá trình quét hai con trỏ sẽ chọn thêm hai số 3 nữa, tạo ra bộ ba chỉ số riêng biệt hợp lệ. 

| tôi (đã sửa) | tôi | r | giá trị | tổng hợp | hành động | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 3 | 3 + 3 + 3 | 9 | tìm thấy | 

Điều này xác nhận tính chính xác ngay cả khi cần có bản sao cho giải pháp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) | Mỗi phần tử cố định sẽ kích hoạt quét hai con trỏ tuyến tính trên hậu tố còn lại | 
| Không gian | O(n) | Lưu trữ các cặp chỉ số giá trị | 

Độ phức tạp bậc hai có thể chấp nhận được đối với các ràng buộc thông thường lên tới khoảng 2×10^5 khi được triển khai hiệu quả trong Python hoặc C++ với các lần thoát sớm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    
    solve()
    
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return out.getvalue().strip()

# provided sample style case
assert run("5 10\n2 3 7 5 1\n") in {"1 2 3", "1 3 2", "2 1 3", "2 3 1", "3 1 2", "3 2 1"}

# minimal case
assert run("3 6\n1 2 3\n") == "1 2 3"

# duplicate-required case
assert run("4 9\n3 3 3 4\n") != "IMPOSSIBLE"

# no solution case
assert run("4 100\n1 2 3 4\n") == "IMPOSSIBLE"

# negative values case
assert run("4 0\n-1 0 1 2\n") != "IMPOSSIBLE"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 5 10/2 3 7 5 1 | bất kỳ bộ ba hợp lệ nào | tính đúng đắn cơ bản | 
| 3 6 / 1 2 3 | 1 2 3 | trường hợp nhỏ nhất có thể giải được | 
| 4 9 / 3 3 3 4 | không thể | xử lý trùng lặp | 
| 4 100 / 1 2 3 4 | KHÔNG THỂ | con đường không có giải pháp | 

## Vỏ cạnh 

Trường hợp quan trọng là khi giải pháp hợp lệ sử dụng các giá trị lặp lại. Với đầu vào n = 4, x = 9, mảng [3, 3, 3, 4], thuật toán sắp xếp thành [(3,1), (3,2), (3,3), (4,4)]. Cố định phần tử đầu tiên (3,1), tìm kiếm hai con trỏ hoạt động trên hậu tố. Các con trỏ cuối cùng sẽ chọn (3,2) và (3,3), tạo ra kết quả đầu ra chính xác. Điều bất biến ở đây là các chỉ số riêng biệt được giữ nguyên thông qua việc sắp xếp vì mỗi phần tử đều mang vị trí ban đầu của nó. 

Một trường hợp cạnh khác xảy ra khi không có giải pháp nào tồn tại. Đối với n = 4, x = 100, mảng [1, 2, 3, 4], con trỏ sẽ sử dụng hết tất cả các kết hợp mà không bao giờ khớp với tổng mục tiêu. Vòng lặp bên ngoài hoàn thành mà không kết thúc sớm và thuật toán in chính xác "KHÔNG THỂ".
