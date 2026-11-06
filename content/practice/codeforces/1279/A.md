---
problem: 1279A
contest_id: 1279
problem_index: A
name: "New Year Garland"
contest_name: "Educational Codeforces Round 79 (Rated for Div. 2)"
rating: 900
tags: ["math"]
answer: passed_samples
verified: true
solve_time_s: 620
date: 2026-06-14
model: gpt-5-5
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a2da27f-fe24-83ec-a977-92dd825c2d7e
---

#CF 1279A - Vòng Hoa Năm Mới 

**Xếp hạng:** 900 
**Thẻ:** toán 
**Mẫu:** gpt-5-5 
**Thời gian giải:** 10 phút 20s 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a2da27f-fe24-83ec-a977-92dd825c2d7e 

--- 

##Giải pháp 

## Hiểu vấn đề 

Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Mỗi trường hợp thử nghiệm mô tả ba đống đèn màu: đỏ, lục và lam. Nhiệm vụ là quyết định xem liệu chúng ta có thể sắp xếp tất cả các đèn thành một hàng sao cho không có hai đèn liền kề nào có cùng màu, trong khi mỗi đèn sử dụng đúng một lần. 

Nói cách khác, chúng ta muốn biết liệu có tồn tại hoán vị của một tập hợp ba ký hiệu sao cho các ký hiệu bằng nhau không bao giờ chạm vào nhau hay không. Mỗi trường hợp thử nghiệm chỉ là một kiểm tra tính khả thi cho việc sắp xếp lại như vậy. 

Các ràng buộc có độ lớn lớn đối với số lượng, lên tới$10^9$, nhưng số lượng trường hợp thử nghiệm ít, nhiều nhất là 100. Điều này ngay lập tức loại trừ bất kỳ phương pháp nào cố gắng xây dựng trình tự một cách rõ ràng hoặc mô phỏng các vị trí theo từng bước. Bất kỳ mô phỏng dựa trên công trình nào cũng sẽ yêu cầu thời gian tỷ lệ thuận với tổng số đèn, trong trường hợp xấu nhất có thể lên tới hàng tỷ. 

Cấu trúc của bài toán gợi ý rằng chỉ có tần số tương đối mới quan trọng. Kích thước tuyệt đối không liên quan ngoại trừ việc so sánh giữa đống lớn nhất và phần còn lại. 

Một vài mẫu cạnh đáng để suy nghĩ sớm. 

Nếu một màu chiếm ưu thế nhiều, ví dụ 1 đỏ, 10 xanh lá cây, 2 xanh lam, thì sau khi đặt tất cả các đèn không phải xanh lục, không có cách nào tránh việc đặt các đèn xanh liên tiếp, vì không có đủ “khoảng trống” do các màu khác tạo ra. 

Nếu tất cả các số đếm đều bằng nhau, chẳng hạn như 3 3 3, thì rõ ràng tồn tại một sự sắp xếp xen kẽ hợp lệ. 

Nếu hai màu nhỏ và một màu lớn hơn một chút, chẳng hạn như 2 1 1, chúng ta vẫn có thể xen kẽ chúng bằng cách luôn tách màu lớn hơn với các màu khác. 

Trường hợp lỗi nghiêm trọng là khi số lượng lớn nhất quá lớn so với tổng của hai số còn lại. 

## Phương pháp tiếp cận 

Một chiến lược bạo lực sẽ cố gắng xây dựng sự sắp xếp một cách rõ ràng. Một cách là bắt đầu với một dãy trống và liên tục đặt màu đèn còn lại khác với màu được đặt cuối cùng. Ở mỗi bước, chúng tôi thử tất cả các màu có sẵn ngoại trừ màu trước đó và chọn một màu nếu có thể. 

Công trình tham lam này có thể hoạt động, nhưng ngay cả khi được thực hiện cẩn thận, nó vẫn hoạt động theo một trình tự có độ dài là$r + g + b$, có thể lớn bằng$3 \cdot 10^9$. Điều đó làm cho nó hoàn toàn không khả thi. Ngay cả việc lưu trữ hoặc lặp lại một chuỗi như vậy là không thể. 

Quan sát quan trọng là toàn bộ vấn đề bị chi phối bởi một ràng buộc duy nhất: chúng ta cần có đủ “dấu phân cách” để ngăn các màu giống hệt nhau trở nên liền kề. Hai cọc nhỏ hơn cùng nhau đóng vai trò là dải phân cách cho cọc lớn nhất. 

Nếu chúng ta sắp xếp số đếm sao cho$a \ge b \ge c$, chúng ta có thể nghĩ đến việc đặt màu lớn nhất trước. Giữa các vị trí liên tiếp của màu chủ đạo này, chúng ta cần ít nhất một đèn không chủ đạo. Điều đó có nghĩa là chúng ta cần ít nhất$a - 1$tách biệt các vị trí có sẵn, đó chính xác là những gì$b + c$cung cấp. 

Vì vậy, điều kiện giảm xuống xem liệu cọc lớn nhất có vượt quá tổng số của hai cọc còn lại nhiều hơn một hay không. 

Chính xác hơn, tính khả thi được giữ vững khi$a \le b + c + 1$. 

Bất đẳng thức duy nhất này nắm bắt tất cả các phép xen kẽ hợp lệ và mọi thứ khác theo sau nó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Xây dựng tham lam | O(r+g+b) | O(r+g+b) | Quá chậm | 
| Sắp xếp + kiểm tra bất đẳng thức | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Đối với mỗi trường hợp thử nghiệm, chúng tôi quyết định liệu một sự sắp xếp hợp lệ có tồn tại hay không chỉ bằng cách suy luận về số lượng. 

1. Đọc ba số nguyên biểu thị số lượng đèn của mỗi màu. 

Thứ tự ban đầu không quan trọng vì chúng tôi sẽ bình thường hóa nó. 
2. Sắp xếp ba giá trị sao cho số lớn nhất trở thành$a$, còn hai số còn lại là$b$Và$c$. 

Bước này tách biệt màu có nhiều khả năng gây ra sự cố liền kề nhất. 
3. Kiểm tra xem số lượng lớn nhất có quá lớn so với các số khác hay không. 

Cụ thể, hãy kiểm tra xem$a > b + c + 1$. 

Nếu bất đẳng thức này đúng, chúng ta trả về "Không". Lý do là ngay cả khi chúng ta xen kẽ hoàn hảo thì hai màu nhỏ hơn cũng chỉ có thể tạo ra$b + c$khoảng trống, không đủ để phân tách tất cả các lần xuất hiện của màu lớn nhất. 
4. Nếu không, trả về "Có". 

Trong trường hợp này, các màu nhỏ hơn đủ để phân tách các lần xuất hiện của màu chủ đạo và chúng ta luôn có thể xây dựng một chuỗi hợp lệ bằng cách phân phối chúng giữa các khối lớn. 

### Tại sao nó hoạt động 

Vấn đề sắp xếp hoàn toàn được kiểm soát bởi số lượng “khe” có thể được tạo ra để phân tách các ký hiệu giống hệt nhau. Bất kỳ trình tự hợp lệ nào cũng phải đặt các lần xuất hiện của màu thường xuyên nhất ở các vị trí riêng biệt và mỗi lần phân tách cần có ít nhất một đèn có màu khác. Tổng số dấu phân cách có sẵn chính xác là tổng của hai màu còn lại. Nếu tổng đó quá nhỏ thì việc va chạm là không thể tránh khỏi dù có sắp xếp thế nào đi chăng nữa. Nếu nó đủ lớn, chúng ta luôn có thể xen kẽ một cách tham lam mà không bao giờ ép buộc hai người hàng xóm giống hệt nhau. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    r, g, b = map(int, input().split())
    a, b1, c = sorted([r, g, b], reverse=True)
    if a > b1 + c + 1:
        print("No")
    else:
        print("Yes")
```Việc thực hiện phản ánh trực tiếp điều kiện toán học. Việc sắp xếp đảm bảo chúng tôi luôn coi màu thường xuyên nhất là nút thắt cổ chai tiềm ẩn. Sự so sánh`a > b1 + c + 1`mã hóa điều kiện không thể xảy ra bắt nguồn từ việc đếm khoảng cách. Mọi thứ khác đều có thời gian không đổi cho mỗi trường hợp thử nghiệm, vì vậy giải pháp cực kỳ hiệu quả ngay cả ở kích thước đầu vào tối đa. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào mẫu: 

đầu vào:```
3
3 3 3
1 10 2
2 1 1
```Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

Đối với trường hợp đầu tiên: 

| r | g | b | được sắp xếp (a, b, c) | a vs b+c+1 | kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 3 | 3 | 3 | (3, 3, 3) | 3 6 | Có | 

Số lượng được cân bằng nên chúng ta có thể luân phiên các màu một cách tự do mà không cần phải có sự liền kề. 

Đối với trường hợp thứ hai: 

| r | g | b | được sắp xếp (a, b, c) | a vs b+c+1 | kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 10 | 2 | (10, 2, 1) | 10 > 4 | Không | 

Ở đây, màu chủ đạo (màu xanh lá cây) không thể được phân tách đủ nhiều lần bằng đèn đỏ và đèn xanh. 

Đối với trường hợp thứ ba: 

| r | g | b | được sắp xếp (a, b, c) | a vs b+c+1 | kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 2 | 1 | 1 | (2, 1, 1) | 2 3 | Có | 

Mặc dù một màu lớn hơn nhưng hai màu còn lại vẫn cung cấp đủ điểm phân tách. 

Những ví dụ này cho thấy hạn chế thực sự duy nhất là liệu màu đa số có thể được giãn cách bằng cách sử dụng các màu còn lại hay không. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp thử nghiệm chỉ yêu cầu sắp xếp ba số và một phép so sánh | 
| Không gian | O(1) | Chỉ sử dụng bộ nhớ bổ sung liên tục | 

Thuật toán chạy trong thời gian không đổi cho mỗi trường hợp thử nghiệm, đủ nhanh để$t \le 100$, mặc dù bản thân các giá trị có thể lớn bằng$10^9$. 

## Trường hợp thử nghiệm```python
import sys, io

def solve():
    import sys
    input = sys.stdin.readline
    t = int(input())
    out = []
    for _ in range(t):
        r, g, b = map(int, input().split())
        a, b1, c = sorted([r, g, b], reverse=True)
        out.append("Yes" if a <= b1 + c + 1 else "No")
    return "\n".join(out)

def run(inp: str) -> str:
    return solve.__wrapped__() if hasattr(solve, "__wrapped__") else __import__("builtins").exec("pass")  # placeholder

# Re-define properly for testing
def run(inp: str) -> str:
    import sys, io
    backup = sys.stdin
    sys.stdin = io.StringIO(inp)
    try:
        t = int(sys.stdin.readline())
        res = []
        for _ in range(t):
            r, g, b = map(int, sys.stdin.readline().split())
            a, b1, c = sorted([r, g, b], reverse=True)
            res.append("Yes" if a <= b1 + c + 1 else "No")
        return "\n".join(res)
    finally:
        sys.stdin = backup

# provided samples
assert run("3\n3 3 3\n1 10 2\n2 1 1\n") == "Yes\nNo\nYes"

# all equal
assert run("1\n5 5 5\n") == "Yes"

# impossible dominant
assert run("1\n1 10 1\n") == "No"

# edge: barely possible
assert run("1\n4 1 1\n") == "Yes"

# minimum
assert run("1\n1 1 1\n") == "Yes"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 3 3 | Có | trường hợp cân bằng | 
| 1 10 1 | Không | tràn chiếm ưu thế | 
| 4 1 1 | Có | trường hợp đẳng thức biên | 
| 1 1 1 | Có | cấu trúc hợp lệ tối thiểu | 

## Vỏ cạnh 

Một trường hợp tinh vi là khi cọc lớn nhất nhiều hơn tổng của các cọc khác đúng một đơn vị. Ví dụ: 4 1 1 sắp xếp thành (4, 1, 1). Đây$b + c = 2$, Và$a = 4$, Vì thế$a > b + c + 1$trở thành$4 > 3$, điều đó đúng, nên điều đó là không thể. Thuật toán từ chối nó một cách chính xác vì không có đủ dấu phân cách. 

Một trường hợp khác là khi tất cả số đếm đều bằng nhau. Với 5 5 5, điều kiện trở thành$5 \le 10 + 1$, điều này đúng một cách tầm thường. Cấu trúc đảm bảo rằng sự đan xen theo chu kỳ tồn tại và bất đẳng thức nắm bắt được điều đó mà không cần xây dựng nó. 

Ngay cả khi hai màu cực kỳ nhỏ so với một màu lớn, quy tắc sẽ phân tách rõ ràng các trường hợp có thể giải được và không thể giải được hoàn toàn thông qua so sánh số học, tránh mọi nhu cầu sắp xếp rõ ràng.
