---
title: "CF 103478A - \u76ae\u5361\u4e18\u4e0e Codeforce"
description: "Chúng tôi được cấp nhiều tài khoản Codeforces, mỗi tài khoản bắt đầu bằng giá trị xếp hạng riêng. Theo thời gian, một chuỗi các cuộc thi diễn ra và mỗi cuộc thi đóng góp một sự thay đổi xếp hạng cố định. Đối với mỗi cuộc thi, chúng ta phải chọn chính xác một tài khoản để tham gia."
date: "2026-07-03T06:34:37+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103478
codeforces_index: "A"
codeforces_contest_name: "The 16-th Beihang University Collegiate Programming Contest (BCPC 2021) - Final"
rating: 0
weight: 103478
solve_time_s: 45
verified: true
draft: false
---

[CF 103478A - \u76ae\u5361\u4e18\u4e0e Codeforces](https://codeforces.com/problemset/problem/103478/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp nhiều tài khoản Codeforces, mỗi tài khoản bắt đầu bằng giá trị xếp hạng riêng. Theo thời gian, một chuỗi các cuộc thi diễn ra và mỗi cuộc thi đóng góp một sự thay đổi xếp hạng cố định. Đối với mỗi cuộc thi, chúng ta phải chọn chính xác một tài khoản để tham gia. Khi một tài khoản được chọn tham gia cuộc thi, chỉ riêng tài khoản đó sẽ nhận được thay đổi xếp hạng cho cuộc thi đó, trong khi tất cả các tài khoản khác không thay đổi. 

Sau khi tất cả các cuộc thi được chỉ định cho các tài khoản, mỗi tài khoản sẽ tích lũy một số tập hợp con các thay đổi về xếp hạng. Giá trị cuối cùng của mỗi tài khoản là xếp hạng ban đầu cộng với tổng tất cả các khoản đóng góp cho cuộc thi được chỉ định cho tài khoản đó. Mục tiêu của chúng tôi là phân phối các cuộc thi giữa các tài khoản sao cho xếp hạng cuối cùng tối đa giữa tất cả các tài khoản càng lớn càng tốt. 

Cấu trúc của vấn đề gợi ý một sự cân bằng: tập trung nhiều lợi ích tích cực vào một tài khoản sẽ tăng tối đa, nhưng các giá trị âm có thể kéo tài khoản xuống. Vì chúng tôi đang tối đa hóa giá trị cuối cùng tối đa trên tất cả các tài khoản nên chúng tôi đang cố gắng xây dựng một tài khoản “tốt nhất” một cách hiệu quả bằng cách chọn một tập hợp con các cuộc thi cho tài khoản đó, trong khi các tài khoản khác đóng vai trò là phần chìm cho các cuộc thi còn lại. 

Các ràng buộc cho phép tối đa 100.000 tài khoản và 100.000 cuộc thi. Bất kỳ giải pháp nào cố gắng liệt kê các bài tập hoặc thậm chí lập trình động trên các tập hợp con đều không thể thực hiện được. Một giải pháp phải gần tuyến tính hoặc tuyến tính, xung quanh O(n + m) hoặc O((n + m) log n). 

Một trường hợp khó phát hiện khi tất cả di đều âm. Một chiến lược tham lam ngây thơ luôn gán những điểm tích cực cho tài khoản tốt nhất hiện tại và những điểm tiêu cực ở nơi khác có thể thất bại nếu nó không giải thích rõ ràng thực tế là việc phân bổ những điểm tiêu cực trên các tài khoản vẫn ảnh hưởng gián tiếp đến mức tối đa toàn cầu bằng cách hạ thấp các ứng cử viên thay thế. 

Một trường hợp góc khác là khi tất cả di đều dương. Trong trường hợp đó, chiến lược tối ưu là xếp tất cả các cuộc thi vào một tài khoản duy nhất, nhưng điều này không rõ ràng ngay lập tức nếu một người tham lam nghĩ đến việc phân công cho mỗi cuộc thi mà không xem xét mục tiêu cuối cùng. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực là xem xét mọi nhiệm vụ có thể có của mỗi cuộc thi cho một trong n tài khoản. Đối với mỗi nhiệm vụ, chúng tôi tính toán xếp hạng cuối cùng và theo dõi giá trị tối đa trên các tài khoản. Điều này dẫn đến n lựa chọn cho mỗi cuộc thi, tạo ra n^m bài tập có thể thực hiện được. Ngay cả đối với những giá trị nhỏ, điều này vẫn lớn về mặt thiên văn và hoàn toàn không khả thi. 

Quan sát quan trọng là chỉ có một tài khoản thực sự quan trọng đối với câu trả lời cuối cùng, cụ thể là tài khoản đạt mức tối đa sau tất cả các lần cập nhật. Giả sử chúng tôi sửa tài khoản nào sẽ trở thành mức tối đa cuối cùng. Khi đó, mọi nhiệm vụ cuộc thi không thuộc về tài khoản này đều không liên quan ngoại trừ việc nó ảnh hưởng gián tiếp đến tài khoản đó như thế nào. Do đó, đối với tài khoản i đã chọn, chúng ta muốn tối đa hóa giá trị cuối cùng của nó bằng cách gán cho nó càng nhiều di có lợi càng tốt. 

Tuy nhiên, chúng tôi không bắt buộc phải gán tất cả các cuộc thi vào tài khoản đó. Bất kỳ cuộc thi nào được chỉ định ở nơi khác đều không đóng góp cho i, vì vậy để tối đa hóa i, chúng ta rõ ràng muốn gán mọi cuộc thi cho i. Không có hạn chế ngăn chặn điều này. Do đó, đối với bất kỳ tài khoản cố định i nào, giá trị cuối cùng tốt nhất có thể có của nó chỉ đơn giản là ri + sum(di trên tất cả các cuộc thi). 

Điều này có nghĩa là mọi tài khoản đều có thể độc lập đạt được tổng số tiền tăng(di) như nhau. Do đó, sự khác biệt duy nhất giữa các tài khoản là giá trị ban đầu ri của chúng. Mức tối đa cuối cùng tốt nhất có thể đạt được bằng cách lấy tài khoản có xếp hạng ban đầu lớn nhất và tham gia tất cả các cuộc thi.

Bước lý luận tinh tế là nhận ra rằng vì tất cả di luôn được áp dụng cho chính xác một tài khoản và chúng tôi đang tối đa hóa mức tối đa cho các tài khoản, nên không bao giờ có lý do để phân chia các cuộc thi giữa các tài khoản theo cách làm giảm mức đóng góp cho ứng cử viên tối đa hiện tại. Tập trung mọi thứ vào tài khoản ban đầu mạnh nhất sẽ chiếm ưu thế trong tất cả các lựa chọn thay thế. 

Bây giờ chúng ta có thể rút gọn bài toán thành một phép tính đơn giản: tính tổng của di, sau đó cộng nó với max ri. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Phân công vũ lực | O(n^m) | O(n) | Quá chậm | 
| Tổng hợp tối ưu | O(n + m) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc n và m, tiếp theo là mảng xếp hạng ban đầu ri và mảng thi thay đổi di. 
2. Tính giá trị lớn nhất trong số tất cả các xếp hạng ban đầu. Đây là điểm khởi đầu mạnh nhất trong số tất cả các tài khoản. Bất kỳ giải pháp tối ưu nào cũng phải kết thúc ở một khía cạnh nào đó và việc bắt đầu từ đường cơ sở tốt nhất luôn chiếm ưu thế. 
3. Tính tổng của tất cả di. Điều này thể hiện tổng mức tăng xếp hạng hiện có sẽ được đưa vào hệ thống trong tất cả các cuộc thi. 
4. Cộng tổng số di vào mức xếp hạng ban đầu tối đa. Điều này tương ứng với việc chỉ định mọi cuộc thi cho tài khoản đã có giá trị ban đầu cao nhất, đảm bảo rằng tài khoản này tích lũy toàn bộ lợi nhuận. 
5. Xuất ra giá trị kết quả. 

Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, mỗi cuộc thi đóng góp chính xác một di vào đúng một tài khoản. Do đó, trên tất cả các tài khoản cộng lại, tổng giá trị gia tăng là cố định và bằng tổng (di). Vì chúng tôi đang tối đa hóa giá trị tài khoản cuối cùng tối đa nên chúng tôi muốn tập trung càng nhiều tổng số cố định này vào một tài khoản. Bất kỳ sự phân phối nào phân chia các khoản đóng góp trên nhiều tài khoản chỉ có thể làm giảm giá trị cao nhất có thể đạt được của bất kỳ tài khoản nào so với việc tập trung tất cả các khoản đóng góp. Do đó, chiến lược tối ưu là chỉ định tất cả các khoản đóng góp vào tài khoản có giá trị ban đầu cao nhất, đưa ra đáp án cuối cùng max(ri) + sum(di). 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    r = list(map(int, input().split()))
    d = list(map(int, input().split()))
    
    best = max(r)
    total = sum(d)
    
    print(best + total)

if __name__ == "__main__":
    solve()
```Việc triển khai rất đơn giản: chúng tôi quét các xếp hạng ban đầu một lần để tìm mức tối đa và quét các vùng đồng bằng của cuộc thi một lần để tính tổng của chúng. Kết quả là tổng của hai giá trị này. Không cần phân loại hoặc mô phỏng. 

Một lỗi phổ biến là cố gắng mô phỏng nhiệm vụ cho mỗi cuộc thi hoặc theo dõi cập nhật cho mỗi tài khoản, điều này là không cần thiết và gây rủi ro cho TLE. Một sai lầm khác là cố gắng phân phối di dương và di âm riêng biệt, nhưng điều này bỏ qua rằng tổng số tiền là bất biến và không phụ thuộc vào phép gán. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào: 

n = 2, m = 2 

r = [1500, 1500] 

d = [300, -300] 

Chúng tôi tính toán: 

| Bước | Đánh giá tốt nhất | Tổng d | Câu trả lời hiện tại | 
| --- | --- | --- | --- | 
| Ban đầu | 1500 | 0 | 1500 | 
| Sau tổng | 1500 | 0 | 1500 | 

Kết quả cuối cùng là 1500. 

Điều này cho thấy một trường hợp mà lãi và lỗ bị loại bỏ. Mặc dù một cuộc thi là tích cực và một cuộc thi là tiêu cực, cả hai đều phải được chỉ định ở đâu đó và tổng hiệu ứng ròng là bằng không. Mức tối đa tốt nhất có thể đạt được vẫn là xếp hạng tốt nhất ban đầu. 

### Ví dụ 2 

đầu vào: 

n = 3, m = 3 

r = [100, 200, 50] 

d = [10, 20, 30] 

| Bước | Đánh giá tốt nhất | Tổng d | Câu trả lời hiện tại | 
| --- | --- | --- | --- | 
| Ban đầu | 200 | 0 | 200 | 
| Sau tổng | 200 | 60 | 260 | 

Kết quả cuối cùng là 260. 

Điều này xác nhận rằng việc tập trung tất cả các khoản đóng góp tích cực vào tài khoản ban đầu mạnh nhất sẽ mang lại giá trị cuối cùng tối đa có thể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Một lượt để tính toán xếp hạng tối đa và một lượt để tính tổng delta | 
| Không gian | O(1) | Chỉ có một số biến tích lũy được sử dụng | 

Các ràng buộc cho phép tổng số đầu vào lên tới 200.000 và quét tuyến tính dễ dàng phù hợp với giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, m = map(int, input().split())
    r = list(map(int, input().split()))
    d = list(map(int, input().split()))
    return str(max(r) + sum(d))

# provided sample
assert run("2 2\n1500 1500\n300 -300\n") == "1500"

# minimum case
assert run("1 1\n5\n10\n") == "15"

# all negative updates
assert run("3 3\n100 200 150\n-1 -2 -3\n") == str(200 - 6)

# all positive updates
assert run("2 4\n10 20\n1 2 3 4\n") == str(20 + 10)

# mixed ratings
assert run("3 2\n-5 0 5\n100 -50\n") == str(5 + 50)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tài khoản duy nhất | xử lý tập hợp tầm thường | tính đúng đắn của trường hợp cơ sở | 
| tất cả âm di | xác minh hành vi hủy bỏ | xử lý tiêu cực | 
| tất cả tích cực di | xác minh tích lũy đầy đủ | tham lam tập trung | 
| giá trị ri hỗn hợp | đảm bảo lựa chọn tối đa chính xác | khởi tạo đúng đắn | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi tất cả di đều âm. Ví dụ: nếu ri = [100, 90] và di = [-10, -20], thuật toán sẽ tính max ri = 100 và tổng di = -30, tạo ra 70. Về mặt khái niệm, việc thực thi gán tất cả các bản cập nhật cho tài khoản tốt nhất, mang lại 100 - 30. Mọi nỗ lực phân phối số âm trên các tài khoản đều không cải thiện mức tối đa vì mọi số âm vẫn phải được chỉ định ở đâu đó. 

Một trường hợp đặc biệt khác là khi chỉ có một tài khoản. Trong trường hợp này, tất cả các cuộc thi phải được gán cho nó, do đó câu trả lời giảm xuống còn r1 + sum(di). Thuật toán xử lý điều này một cách tự nhiên vì max(r) là r1. 

Trường hợp cạnh cuối cùng là khi tất cả di bằng 0. Hệ thống không làm gì cả và câu trả lời chỉ đơn giản là xếp hạng ban đầu tối đa. Thuật toán giảm chính xác vì sum(di) bằng 0 và mức tối đa không thay đổi.
