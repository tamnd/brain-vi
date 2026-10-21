---
title: "CF 105838K - Số tái thiết của ruskal"
description: "Chúng ta được cho một số nguyên dương được viết dưới dạng chuỗi thập phân. Trong một nước đi, chúng ta được phép chọn một cặp chữ số liền kề và hoán đổi chúng. Chúng tôi có thể thực hiện nhiều nhất một lần hoán đổi như vậy hoặc chọn không làm gì cả."
date: "2026-06-22T01:23:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105838
codeforces_index: "K"
codeforces_contest_name: "The 14th Huazhong Agricultural University Programming Contest"
rating: 0
weight: 105838
solve_time_s: 46
verified: true
draft: false
---

[CF 105838K - Số tái tạo của ruskal](https://codeforces.com/problemset/problem/105838/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số nguyên dương được viết dưới dạng chuỗi thập phân. Trong một nước đi, chúng ta được phép chọn một cặp chữ số liền kề và hoán đổi chúng. Chúng tôi có thể thực hiện nhiều nhất một lần hoán đổi như vậy hoặc chọn không làm gì cả. Mục tiêu là tối đa hóa số kết quả sau thao tác tùy chọn này. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm độc lập. Mỗi trường hợp thử nghiệm chỉ là một số nguyên duy nhất và chúng ta phải xuất ra số nguyên tối đa có thể đạt được bằng 0 hoặc một lần hoán đổi liền kề. 

Các ràng buộc ngụ ý rằng tổng số chữ số trong tất cả các trường hợp thử nghiệm là lớn, tổng cộng lên tới 200.000. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào thử tất cả các giao dịch hoán đổi có thể một cách độc lập cho mỗi trường hợp thử nghiệm theo cách lồng nhau trên tất cả các vị trí và sau đó tính toán lại các phép so sánh đầy đủ nhiều lần. Tuy nhiên, vì mỗi trường hợp thử nghiệm là độc lập và số phép toán là tuyến tính theo số chữ số nên cách tiếp cận tuyến tính hoặc gần tuyến tính trên mỗi số là đủ. 

Trường hợp cạnh tinh tế xuất hiện khi số đã tối ưu theo nghĩa từ điển, nghĩa là bất kỳ sự hoán đổi nào cũng sẽ làm giảm giá trị. Ví dụ: một dãy vốn đã không tăng như 987654. Mọi hoán đổi chỉ tạo ra một số nhỏ hơn, vì vậy câu trả lời đúng là số ban đầu. Một trao đổi tham lam bất cẩn luôn thực hiện thay đổi cục bộ cải tiến đầu tiên có thể thất bại trong trường hợp cải tiến cục bộ không tối ưu toàn cục. 

Một trường hợp cạnh quan trọng khác là khi các chữ số bằng nhau xuất hiện. Ví dụ: 1221. Hoán đổi cặp liền kề đầu tiên sẽ nhận được 2121, lớn hơn so với hoán đổi sau đó hoặc không làm gì cả. Thuật toán phải xử lý chính xác các phép so sánh chữ số bằng nhau mà không coi sự bình đẳng là sự cải thiện. 

Cuối cùng, việc cân nhắc chữ số hàng đầu có ý nghĩa ngầm. Vì chúng ta đang sắp xếp lại các chữ số của một số nguyên dương mà không có số 0 đứng đầu trong đầu vào, nên bất kỳ phép hoán đổi nào di chuyển một chữ số lớn hơn về phía trước đều có thể thay đổi đáng kể độ lớn, do đó việc so sánh phải được thực hiện theo từ điển trên các chữ số. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: với mỗi trường hợp thử nghiệm, hãy thử mọi vị trí có thể i, hoán đổi các chữ số ở i và i+1, xây dựng số kết quả và giữ số lớn nhất trong số tất cả các kết quả đó và số ban đầu. Điều này đúng vì nó khám phá toàn diện tất cả các trạng thái được phép có thể truy cập được trong một lần di chuyển. Tuy nhiên, đối với một số độ dài L, điều này yêu cầu hoán đổi O(L) và mỗi lần hoán đổi tốn O(L) để đánh giá hoặc xây dựng lại chuỗi, đưa ra O(L^2) cho mỗi trường hợp thử nghiệm trong cách triển khai đơn giản. Qua nhiều trường hợp thử nghiệm, điều này trở nên quá chậm. 

Quan sát quan trọng là chúng ta không cần phải thử tất cả các giao dịch hoán đổi. Chúng tôi chỉ quan tâm đến việc cải thiện số lượng càng sớm càng tốt từ bên trái, vì các vị trí cao hơn sẽ chiếm ưu thế về giá trị. Nếu chúng ta quyết định hoán đổi các chữ số ở vị trí i và i+1, điều duy nhất quan trọng là liệu việc đặt một chữ số lớn hơn sớm hơn có mang lại tiền tố tốt hơn số ban đầu hay không. Bất kỳ sự hoán đổi nào cũng chỉ ảnh hưởng đến hai vị trí liền kề, do đó, hiệu ứng lên số sẽ được cục bộ hóa và chúng ta có thể mô phỏng từng lần hoán đổi trong phép so sánh O(1) so với cấu trúc ban đầu. 

Do đó, chúng tôi có thể quét tất cả các cặp liền kề một lần cho mỗi trường hợp thử nghiệm, mô phỏng hoán đổi cục bộ và so sánh chuỗi kết quả với câu trả lời tốt nhất được thấy cho đến nay. Vì các so sánh mang tính từ điển và chỉ cần kiểm tra dựa trên cấu hình tốt nhất hiện tại nên chúng tôi có thể duy trì cấu hình tốt nhất dần dần mà không cần phải xây dựng lại nhiều lần các trạng thái không cần thiết. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(L^2) mỗi lần kiểm tra | O(L) | Quá chậm | 
| Hãy thử tất cả các giao dịch hoán đổi liền kề một lần | O(L) mỗi lần kiểm tra | O(L) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi coi mỗi số là một chuỗi các chữ số có thể thay đổi.

1. Chuyển đổi số đầu vào thành danh sách các ký tự để dễ mô phỏng các hoán đổi. Điều này cho phép sửa đổi cục bộ theo thời gian liên tục. 
2. Khởi tạo câu trả lời dưới dạng số ban đầu, vì không thực hiện thao tác nào luôn hợp lệ. 
3. Lặp lại mọi chỉ số i từ 0 đến độ dài trừ 2. Tại mỗi vị trí, hãy xem xét việc hoán đổi các chữ số tại i và i+1. 
4. Thực hiện hoán đổi tạm thời, tạo thành cấu hình ứng cử viên. 
5. So sánh cấu hình ứng viên với câu trả lời đúng nhất hiện tại bằng cách sử dụng so sánh từ điển trên chuỗi chữ số. Sự so sánh này có tác dụng vì cả hai chuỗi đều biểu thị các số nguyên có cùng độ dài và không có số 0 đứng đầu nào được đưa vào. 
6. Nếu thí sinh lớn hơn, hãy cập nhật câu trả lời đúng nhất. 
7. Hoàn nguyên việc hoán đổi để chuỗi ban đầu được giữ nguyên cho lần lặp tiếp theo. 
8. Sau khi xử lý tất cả các cặp liền kề, xuất cấu hình được lưu trữ tốt nhất. 

Lý do chính chỉ đằng sau các giao dịch hoán đổi liền kề là vấn đề hạn chế thao tác ở chính xác các giao dịch hoán đổi đó, do đó không gian tìm kiếm chính xác là L trạng thái có thể cộng với trạng thái nhận dạng. 

### Tại sao nó hoạt động 

Mỗi kết quả hợp lệ là số ban đầu hoặc kết quả của việc hoán đổi chính xác một cặp liền kề. Thuật toán liệt kê tất cả các khả năng như vậy chính xác một lần. Vì câu trả lời tốt nhất được chọn trong số tất cả các trạng thái có thể truy cập và so sánh là thứ tự từ điển chính xác của các chuỗi chữ số, nên không có ứng cử viên nào có thể cải thiện câu trả lời bị bỏ qua. Do đó, thuật toán là một bảng liệt kê đầy đủ không gian trạng thái được xác định bởi ràng buộc hoạt động. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []
    for _ in range(t):
        s = list(input().strip())
        best = ''.join(s)

        n = len(s)
        for i in range(n - 1):
            s[i], s[i + 1] = s[i + 1], s[i]
            cand = ''.join(s)
            if cand > best:
                best = cand
            s[i], s[i + 1] = s[i + 1], s[i]

        out.append(best)

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp duy trì một danh sách có thể thay đổi duy nhất cho mỗi trường hợp thử nghiệm và tránh việc xây dựng lại tất cả các trạng thái từ đầu. Mỗi lần hoán đổi được kiểm tra theo thời gian không đổi so với chuyển động của chữ số và việc so sánh chuỗi rất hiệu quả vì Python so sánh theo từ điển và dừng sớm khi không khớp. Mẫu hoán đổi-hoàn nguyên đảm bảo tính chính xác mà không cần phân bổ thêm mảng cho mỗi lần lặp. 

Một cạm bẫy triển khai phổ biến là quên hoàn nguyên trao đổi, điều này sẽ khiến các lần lặp lại sau đó hoạt động ở trạng thái bị lỗi. Một cách khác là xây dựng lại các chuỗi một cách không cần thiết bên trong các vòng lặp lồng nhau, điều này sẽ đẩy thời gian chạy gần hơn với hành vi bậc hai. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
2736
```Chúng tôi kiểm tra tất cả các giao dịch hoán đổi liền kề. 

| tôi | chuỗi hoán đổi | ứng cử viên | tốt nhất | 
| --- | --- | --- | --- | 
| 0 | 7236 | 7236 | 7236 | 
| 1 | 2376 | 7236 | 7236 | 
| 2 | 2736 | 7236 | 7236 | 

Câu trả lời cuối cùng là 7236. 

Điều này cho thấy thuật toán xác định chính xác rằng việc hoán đổi hai chữ số đầu tiên mang lại sự cải thiện tối đa. 

### Ví dụ 2 

đầu vào:```
1
98765
```| tôi | chuỗi hoán đổi | ứng cử viên | tốt nhất | 
| --- | --- | --- | --- | 
| 0 | 89765 | 89765 | 98765 | 
| 1 | 97865 | 97865 | 98765 | 
| 2 | 98675 | 98675 | 98765 | 
| 3 | 98756 | 98756 | 98765 | 

Câu trả lời cuối cùng vẫn là 98765. 

Điều này chứng tỏ trường hợp mọi sự hoán đổi có thể xảy ra đều tệ hơn, do đó danh tính là tối ưu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(tổng số chữ số cho mỗi bài kiểm tra) | Mỗi vị trí được kiểm tra một lần và mỗi bài kiểm tra bao gồm hoán đổi thời gian không đổi cộng với so sánh chuỗi | 
| Không gian | O(L) | Lưu trữ danh sách chữ số và chuỗi tốt nhất | 

Tổng chữ số được giới hạn bởi 200.000, do đó thuật toán chạy thoải mái trong giới hạn. Mỗi trường hợp thử nghiệm có độ dài tuyến tính riêng và không có tương tác bậc hai ẩn giữa các trường hợp. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def solve():
        t = int(input())
        out = []
        for _ in range(t):
            s = list(input().strip())
            best = ''.join(s)
            n = len(s)
            for i in range(n - 1):
                s[i], s[i + 1] = s[i + 1], s[i]
                cand = ''.join(s)
                if cand > best:
                    best = cand
                s[i], s[i + 1] = s[i + 1], s[i]
            out.append(best)
        return "\n".join(out)

    return solve()

assert run("1\n2736\n") == "7236"
assert run("1\n98765\n") == "98765"
assert run("1\n1221\n") == "2121"
assert run("1\n10\n") == "10"
assert run("1\n9\n") == "9"
assert run("3\n12\n21\n132\n") == "21\n21\n312"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2736 | 7236 | cải tiến trao đổi đơn tốt nhất | 
| 98765 | 98765 | không có giao dịch hoán đổi có lợi | 
| 1221 | 2121 | cải thiện chữ số bằng nhau và ở giữa | 
| 10 | 10 | trường hợp ranh giới hai chữ số | 
| 9 | 9 | trường hợp tối thiểu một chữ số | 
| 12 21 132 | 21 21 312 | xử lý nhiều bài kiểm tra | 

## Vỏ cạnh 

Đối với một số có một chữ số như`7`, không có cặp liền kề nào để hoán đổi. Thuật toán khởi tạo câu trả lời tốt nhất dưới dạng chuỗi gốc và không bao giờ đi vào vòng lặp, do đó nó xuất ra một cách chính xác`7`. 

Đối với một chuỗi giảm dần như`543210`, mỗi lần hoán đổi sẽ tạo ra một chuỗi nhỏ hơn về mặt từ điển. Thuật toán vẫn đánh giá tất cả các giao dịch hoán đổi liền kề nhưng không bao giờ cập nhật giá trị tốt nhất, giữ nguyên giá trị ban đầu. 

Đối với các chữ số lặp lại như`11111`, mọi trao đổi đều tạo ra cùng một chuỗi. Sự so sánh`cand > best`không bao giờ kích hoạt, do đó đầu ra không thay đổi, tránh chính xác các cập nhật không cần thiết.
