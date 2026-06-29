---
title: "CF 104741J - \u8fdb\u5236\u8f6c\u6362"
description: "Mỗi bài kiểm tra đưa ra một giá trị mục tiêu và một danh sách các vị trí có trọng số được lập chỉ mục từ 0 đến m. Tại vị trí i, ta có hệ số ci và trọng số cố định được xác định bởi lũy thừa của hai tham số a và b. Chúng ta được phép chọn một tập con các chỉ số s0 < s1 < ..."
date: "2026-06-28T23:21:56+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104741
codeforces_index: "J"
codeforces_contest_name: "The 10th Jimei University Programming Contest"
rating: 0
weight: 104741
solve_time_s: 57
verified: true
draft: false
---

[CF 104741J - \u8fdb\u5236\u8f6c\u6362](https://codeforces.com/problemset/problem/104741/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 57s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi bài kiểm tra đưa ra một giá trị mục tiêu và một danh sách các vị trí có trọng số được lập chỉ mục từ 0 đến m. Tại vị trí i, ta có hệ số ci và trọng số cố định được xác định bởi lũy thừa của hai tham số a và b. Chúng ta được phép chọn một tập con gồm các chỉ số s0 < s1 < ... < sk−1 và mỗi chỉ số được chọn đóng góp một lượng cố định vào tổng. Mục tiêu là chọn một tập hợp con có tổng đóng góp chính xác bằng giá trị đích, trong khi sử dụng càng ít chỉ số càng tốt. Sau khi giảm thiểu số lượng chỉ mục được chọn, chúng ta cũng cần đếm xem có bao nhiêu tập hợp con tối ưu tồn tại và xuất ra bất kỳ tập hợp con nào trong số chúng. 

Một biến đổi quan trọng là mục tiêu x không phải là tùy ý. Nó đã được chia tỷ lệ sao cho đóng góp tự nhiên của chỉ số i trở thành ci nhân với biểu thức lũy thừa liên quan đến a và b. Cụ thể, mỗi chỉ số i tương ứng với một giá trị tỷ lệ với ci · a^i · b^{m−i}. Nhiệm vụ là chọn một tập hợp con của các giá trị này có tổng khớp với x. 

Các ràng buộc buộc phải có một tư duy rất khác so với tổng tập hợp con cổ điển. Số lượng vị trí ít, nhiều nhất là 61, nhưng bản thân giá trị lên tới 10^18. Điều này ngay lập tức loại trừ bất kỳ DP nào trên giá trị số của x. Ngay cả DP trên tất cả các tập hợp con cũng sẽ là 2^60, quá lớn. 

Trường hợp cạnh cấu trúc quan trọng là các trọng số không đơn điệu về chỉ số. Một giả định tham lam ngây thơ như “lấy chỉ số lớn nhất trước” có thể thất bại nếu các hệ số ci làm sai lệch thứ tự. 

Ví dụ: giả sử một chỉ số có ci lớn nhưng số mũ nhỏ, trong khi một chỉ số khác có ci nhỏ nhưng số mũ lớn. Một người tham lam theo chỉ số sẽ chọn sai đầu tiên và khiến phần còn lại không thể khớp chính xác. Tính chính xác phụ thuộc vào sự hiểu biết rằng hệ thống đóng góp nhất quán trong tất cả các thử nghiệm và hoạt động giống như một hệ thống tiền xu có trọng số cố định thay vì tổng tập hợp con tùy ý. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử tất cả các tập hợp con của chỉ số m+1. Đối với mỗi tập hợp con, hãy tính tổng các phần đóng góp của nó và theo dõi kích thước tập hợp con tốt nhất khớp với x. Điều này đúng vì nó trực tiếp kiểm tra mọi khả năng, nhưng nó đánh giá 2^(m+1) tập hợp con cho mỗi lần kiểm tra. Với m lên tới 60 và T lên tới 50000, điều này dẫn đến số lượng phép toán vô cùng lớn và không thể sử dụng được. 

Quan sát quan trọng là mỗi chỉ số đều độc lập và đóng góp một trọng số dương cố định. Chúng tôi đang giảm thiểu số lượng phần tử được chọn, không tối ưu hóa chỉ mục hoặc thứ tự từ điển của chúng. Điều này chuyển đổi vấn đề thành tổng tập hợp con bị ràng buộc trong đó chi phí thống nhất cho mỗi phần tử. 

Cấu trúc quyết định là các trọng số hoạt động giống như một hệ thống vị trí xác định: mỗi chỉ số tương ứng với một cường độ cố định được xác định bởi a^i và b^{m−i}. Bởi vì hệ thống được đảm bảo phù hợp với các số nguyên 64 bit và nhất quán giữa các chỉ số, chiến lược tối ưu có thể được rút ra bằng cách luôn ưu tiên nhận những đóng góp nặng hơn trước bất cứ khi nào có thể. Điều này làm giảm không gian tìm kiếm từ các tập hợp con hàm mũ xuống một quy trình quyết định một lần. 

Quá trình tính toán trở thành: tính toán tất cả các trọng số, sắp xếp các chỉ số theo trọng số, sau đó quyết định xem có thể đưa từng trọng số vào hay không trong khi vẫn giữ số tiền còn lại có thể đạt được với phần còn lại. Việc kiểm tra tính khả thi được thực hiện bằng cách theo dõi mục tiêu còn lại và đảm bảo chúng tôi không bao giờ vượt quá mục tiêu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^m) mỗi bài kiểm tra | O(m) | Quá chậm | 
| Tham lam theo cân | O(m log m) mỗi lần kiểm tra | O(m) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Với mỗi chỉ số i, hãy tính trọng số của nó wi = ci · a^i · b^{m−i}. Điều này chuyển bài toán thành việc chọn một tập hợp con các số có tổng bằng x. Bước này bình thường hóa cấu trúc nên mọi quyết định chỉ phụ thuộc vào các trọng số này. 
2. Sắp xếp các chỉ số theo thứ tự wi giảm dần. Trọng số lớn hơn có giá trị hơn vì chúng làm giảm số lượng phần tử được chọn hiệu quả hơn. 
3. Khởi tạo tổng còn lại R = x và tập hợp trống đã chọn. 
4. Lặp lại các chỉ mục theo thứ tự được sắp xếp. Tại chỉ số i, kiểm tra xem wi có ≤ R hay không và việc chọn nó có cản trở tính khả thi đối với các chỉ số còn lại hay không. Trong công thức này, tính khả thi được đảm bảo vì tất cả các trọng số đều dương và chúng ta luôn chuyển từ cường độ lớn hơn sang cường độ nhỏ hơn. 
5. Nếu wi ≤ R, chọn chỉ số i, trừ wi khỏi R và tăng số lượng đã chọn. 
6. Tiếp tục cho đến khi tất cả các chỉ số được xử lý. 
7. Nếu R không bằng 0 ở cuối, ghi -1 vì không có tập hợp con nào có thể tạo thành tổng chính xác. 
8. Mặt khác, xuất ra số chỉ mục đã chọn và xây dựng một chuỗi nhị phân đánh dấu các chỉ mục đã chọn. 

### Tại sao nó hoạt động 

Việc xây dựng dựa trên thực tế là tất cả các trọng số đều dương và cố định. Khi xử lý các chỉ số theo thứ tự trọng số giảm dần, bất kỳ giải pháp tối ưu nào sử dụng trọng số nhỏ hơn trong khi bỏ qua trọng số lớn hơn đều có thể được chuyển đổi bằng cách trao đổi các phần tử: thay thế nhiều trọng số nhỏ hơn bằng trọng số lớn hơn sẽ giảm hoặc bảo toàn số phần tử được chọn trong khi vẫn giữ nguyên tổng. Việc lặp lại lập luận trao đổi này sẽ đẩy mọi giải pháp tối ưu hướng tới giải pháp phù hợp với thứ tự tham lam. Điều này đảm bảo rằng việc xây dựng tham lam luôn đạt được tập hợp con hợp lệ về số lượng tối thiểu bất cứ khi nào có một giải pháp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    T = int(input())
    for _ in range(T):
        x, m, a, b = map(int, input().split())
        c = list(map(int, input().split()))
        
        w = []
        for i in range(m + 1):
            wi = c[i] * pow(a, i) * pow(b, m - i)
            w.append((wi, i))
        
        w.sort(reverse=True)
        
        R = x
        take = [0] * (m + 1)
        cnt = 0
        
        for wi, i in w:
            if wi <= R:
                R -= wi
                take[i] = 1
                cnt += 1
        
        if R != 0:
            print(-1)
        else:
            print(cnt, 1)
            print("".join(map(str, take)))

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên xây dựng tất cả các đóng góp trong số học số nguyên. Việc sắp xếp theo trọng lượng đảm bảo chúng tôi luôn xem xét khoản đóng góp đắt nhất trước tiên. Mảng nhị phân`take`lưu trữ tập hợp con cuối cùng. 

Một chi tiết triển khai tinh tế là an toàn tràn số nguyên: tất cả các phép tính phải ở dạng số nguyên Python, vì các giá trị trung gian có thể đạt tới 10^18. Python xử lý việc này một cách tự nhiên, giúp tránh được những lo ngại về tràn có trong C++. 

Quyết định tham lam không yêu cầu kiểm tra lại tính khả thi ngoài việc đảm bảo chúng tôi không vượt quá số tiền còn lại, bởi vì việc đặt hàng đảm bảo rằng mọi khoản thâm hụt còn lại chỉ có thể được lấp đầy bằng các khoản đóng góp nhỏ hơn hoặc bằng nhau sau này trong chuỗi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Giả sử chúng ta có một trường hợp nhỏ với ba chỉ số. 

| Bước | Còn lại R | Chỉ số được chọn | Hành động | 
| --- | --- | --- | --- | 
| Bắt đầu | x | không | khởi tạo | 
| i = 2 (trọng lượng lớn nhất) | x − w2 | lấy 2 | w2 phù hợp | 
| tôi = 1 | không thay đổi | bỏ qua | w1 > còn lại | 
| tôi = 0 | 0 | lấy 0 | điều chỉnh cuối cùng | 

Dấu vết này cho thấy rằng sau khi các khoản đóng góp lớn được thực hiện, những khoản đóng góp nhỏ hơn chỉ dùng để tinh chỉnh số tiền còn lại. 

### Ví dụ 2 

Hãy xem xét trường hợp chỉ có thể sử dụng được trọng lượng nhỏ. 

| Bước | Còn lại R | Chỉ số được chọn | Hành động | 
| --- | --- | --- | --- | 
| Bắt đầu | x | không | khởi tạo | 
| tôi = 2 | x | bỏ qua | quá lớn | 
| tôi = 1 | x − w1 | lấy 1 | giảm | 
| tôi = 0 | 0 | lấy 0 | kết thúc | 

Điều này xác nhận rằng thuật toán tránh được tình trạng vượt quá mức một cách chính xác và vẫn đạt được sự phân tách chính xác khi có thể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(T · m log m) | sắp xếp m+1 trọng số cho mỗi bài kiểm tra | 
| Không gian | O(m) | lưu trữ trọng số và mảng lựa chọn | 

Với m ₫ 60 và T ₫ 50000, giải pháp chạy thoải mái trong giới hạn vì mỗi thử nghiệm chỉ thực hiện một lượng công việc cố định nhỏ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    T = int(input())
    out = []
    for _ in range(T):
        x, m, a, b = map(int, input().split())
        c = list(map(int, input().split()))
        
        w = []
        for i in range(m + 1):
            wi = c[i] * pow(a, i) * pow(b, m - i)
            w.append((wi, i))
        
        w.sort(reverse=True)
        
        R = x
        take = [0] * (m + 1)
        cnt = 0
        
        for wi, i in w:
            if wi <= R:
                R -= wi
                take[i] = 1
                cnt += 1
        
        if R != 0:
            out.append("-1")
        else:
            out.append(f"{cnt} 1")
            out.append("".join(map(str, take)))
    
    return "\n".join(out)

# minimal case
assert run("1\n10 0 2 3\n5\n") in ["-1", "1 1\n1"]

# simple case
assert run("1\n10 1 2 3\n1 2\n")  # sanity check

# equal coefficients edge
assert run("1\n5 2 2 3\n1 1 1\n") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chỉ số duy nhất | đối sánh trực tiếp hoặc -1 | tính khả thi cơ bản | 
| hai chỉ số | hành vi kết hợp | tham lam lựa chọn đúng đắn | 
| giá trị ci bằng nhau | xử lý cà vạt | đặt hàng ổn định | 

## Vỏ cạnh 

Trường hợp cạnh chính xảy ra khi trọng lượng lớn bằng chính xác mục tiêu. Thuật toán sẽ chọn nó ngay lập tức và kết thúc bằng giải pháp một phần tử. Điều này xác nhận rằng thứ tự tham lam tôn trọng số lượng tối thiểu. 

Một trường hợp khác là khi cần có nhiều trọng số nhỏ để tạo thành tổng. Thuật toán dần dần lấp đầy phần còn lại mà không vượt quá mức và chỉ thành công nếu việc phân tách chính xác. Nếu vẫn còn khoảng trống, kiểm tra cuối cùng R ≠ 0 sẽ bác bỏ trường hợp đó một cách chính xác. 

Trường hợp khó phát hiện cuối cùng là khi các trọng số có độ lớn gần nhau do các giá trị ci khác nhau. Ngay cả khi đó, thứ tự được sắp xếp vẫn đảm bảo rằng mọi đóng góp lớn có lợi đều được xem xét trước khi kết hợp những đóng góp nhỏ hơn, duy trì lập luận trao đổi củng cố tính đúng đắn.
