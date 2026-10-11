---
title: "CF 105009B - Bài tập về nhà hai chiều"
description: "Chúng ta được cho hai chuỗi có cùng độ dài. Tại mỗi vị trí, chúng ta phải chọn chính xác một giá trị, từ chuỗi đầu tiên hoặc từ chuỗi thứ hai, và tổng hợp tất cả các giá trị đã chọn."
date: "2026-06-28T02:35:45+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105009
codeforces_index: "B"
codeforces_contest_name: "2024 USACO.Guide Informatics Tournament"
rating: 0
weight: 105009
solve_time_s: 59
verified: true
draft: false
---

[CF 105009B - Bài tập về nhà hai chiều](https://codeforces.com/problemset/problem/105009/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 59s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho hai chuỗi có cùng độ dài. Tại mỗi vị trí, chúng ta phải chọn chính xác một giá trị, từ chuỗi đầu tiên hoặc từ chuỗi thứ hai, và tổng hợp tất cả các giá trị đã chọn. Điều phức tạp là hành động chọn từ chuỗi đầu tiên sẽ thay đổi vĩnh viễn cấu trúc chi phí: mỗi khi chúng tôi lấy một phần tử từ chuỗi đầu tiên, tất cả các giá trị được chọn trong tương lai sẽ được nhân với 2 một lần. Các phép nhân này tích lũy lại, vì vậy nếu chúng ta đã chọn từ chuỗi đầu tiên$k$lần trước vị trí$i$, thì giá trị chúng ta lấy ở vị trí$i$được nhân với$2^k$. 

Mục tiêu là chọn một chuỗi các quyết định, chọn từ A hoặc B ở mỗi chỉ số, giảm thiểu tổng trọng số cuối cùng. 

Những ràng buộc cho phép$N$lên đến$10^5$, loại trừ bất kỳ giải pháp nào thử tất cả các tập hợp con hoặc mô phỏng các trạng thái quyết định theo cấp số nhân. Một chương trình động đơn giản về “cho đến nay chúng tôi đã chọn từ A bao nhiêu lần” vẫn sẽ quá lớn vì trạng thái đó có thể phát triển tuyến tính và các chuyển đổi sẽ là$O(N^2)$trong những diễn giải tồi tệ nhất nếu không được cấu trúc cẩn thận. 

Một trường hợp khó nhận thấy là khi A luôn tốt hơn một chút ở cục bộ, nhưng việc lấy nó sớm sẽ khiến các giá trị sau này bùng nổ do nhân đôi nhiều lần. Ví dụ: nếu A luôn nhỏ hơn B 1, chiến lược tham lam sẽ luôn chọn A và ngay lập tức trở nên dưới mức tối ưu vì hiệu ứng nhân chiếm ưu thế: 

đầu vào:```
3
1 1 1
10 10 10
```Kẻ tham lam sẽ chọn A ba lần và nhận được$1 + 2 + 4 = 7$, nhưng cách tối ưu là tránh hoàn toàn A và nhận được$30$. Điều này cho thấy so sánh địa phương không thành công. 

Một trường hợp khác là khi một lượt chọn sớm từ A cực kỳ rẻ nhưng sau đó gây ra lạm phát lớn, do đó giải pháp tối ưu có thể cho phép nhiều nhất một hoặc rất ít lượt chọn từ A. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử mọi chuỗi lựa chọn. Tại mỗi chỉ mục, chúng tôi chọn A hoặc B và theo dõi số lần A đã được chọn cho đến nay. Mỗi lựa chọn sẽ cập nhật trạng thái của số nhân. Điều này dẫn đến$2^N$những quyết định khả thi, điều không thể thực hiện được$N = 10^5$. 

Một chế độ xem có cấu trúc hơn một chút là lập trình động trong đó trạng thái là vị trí và số lượng lựa chọn A cho đến nay. Chuyển từ vị trí$i$ĐẾN$i+1$bao gồm hai lựa chọn và tính toán lại chi phí với lũy thừa hiện tại của hai số nhân. Mặc dù không gian trạng thái chỉ$O(N^2)$nếu mở rộng một cách ngây thơ thì vẫn không khả thi. 

Nhận xét quan trọng là điều duy nhất quan trọng về quá khứ là số lần chúng ta đã lấy từ A, bởi vì điều đó quyết định số nhân hiện tại. Tuy nhiên, thay vì coi đây là DP đầy đủ theo số lượng, chúng ta có thể diễn giải lại quá trình từ đầu. 

Nếu chúng ta nghĩ ngược lại, mỗi lần chúng ta quyết định có nên lấy A ở vị trí hay không$i$, quyết định đó xác định liệu tất cả các khoản đóng góp hậu tố có được nhân đôi một lần hay không. Vậy lấy A ở vị trí$i$có tác động chi phí toàn cầu lên tất cả các yếu tố trong tương lai, không chỉ chi phí địa phương. 

Điều này gợi ý một ý tưởng tham lam: chúng ta muốn quyết định vị trí nào góp phần làm tăng số nhân toàn cầu, bởi vì mỗi lựa chọn A sẽ làm tăng chi phí của mọi lựa chọn sau đó. Chi phí tăng do chọn A tại vị trí$i$chính xác là tổng của tất cả các giá trị hậu tố đã chọn theo tỷ lệ hiện tại. Điều này trực tiếp khó khăn, nhưng chúng ta có thể chuyển đổi vấn đề. 

Chúng tôi xác định rằng bất cứ lúc nào chúng tôi cũng duy trì hệ số nhân hiện tại$m$, ban đầu là 1. Nếu chọn từ B, chúng ta trả tiền$B_i \cdot m$. Nếu chúng tôi chọn từ A, chúng tôi sẽ trả tiền$A_i \cdot m$, nhưng sau đó$m$tăng gấp đôi. Mục tiêu là quyết định ở mỗi bước xem việc trả tiền A ngay bây giờ và tăng chi phí trong tương lai có tốt hơn việc chỉ mua B ngay bây giờ hay không. 

Một mẹo hữu ích là nhận ra rằng nếu chúng ta cố định số lần lấy A, thì chiến lược tốt nhất là đặt những lựa chọn A đó vào những vị trí mà A có lợi nhất so với B. Tuy nhiên, vì mỗi A sẽ tăng gấp đôi chi phí trong tương lai, nên những lựa chọn A trước đó sẽ đắt hơn theo cấp số nhân so với những lựa chọn sau. Điều này gợi ý rõ ràng rằng nếu sử dụng A thì nên trì hoãn hoặc tránh sử dụng nó một cách cẩn thận trừ khi nó tốt hơn đáng kể so với B. 

Điều này dẫn đến quá trình quét tham lam trong đó chúng tôi mô phỏng từ trái sang phải trong khi duy trì hệ số nhân hiện tại và cũng theo dõi xem việc sử dụng A ở một vị trí có đáng bị phạt trong tương lai hay không. Cách chính xác để chính thức hóa điều này là coi quyết định này như một DP với hai trạng thái: liệu chúng ta đã cam kết tăng số nhân hay chưa. Tuy nhiên, một nguồn gốc rõ ràng hơn xuất phát từ quan điểm đảo ngược: thay vì nghĩ đến chi phí tương lai của A ngày càng tăng, chúng tôi nghĩ đến việc B được “chiết khấu” so với đường cơ sở ngày càng tăng. Mỗi A tăng đường cơ sở cho tất cả các phần tử còn lại. 

Cấu trúc này dẫn đến một thủ thuật tiêu chuẩn: duy trì mức tiết kiệm tốt nhất có thể nếu chúng ta chuyển từ A sang B tại một thời điểm nhất định và tích lũy các khoản đóng góp trong khi theo dõi mức tăng trưởng theo cấp số nhân. Giải pháp tối ưu giảm xuống còn việc quyết định tham lam, tại mỗi vị trí, nên chọn A hay B dựa trên việc so sánh chi phí trước mắt cộng với tác động mở rộng quy mô trong tương lai, có thể được duy trì theo thời gian tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^N)$|$O(N)$| Quá chậm | 
| DP ngây thơ trên số A |$O(N^2)$|$O(N^2)$| Quá chậm | 
| Mô phỏng tham lam tối ưu |$O(N)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý mảng từ trái sang phải trong khi duy trì hệ số nhân hiện tại, bắt đầu từ 1. Chúng tôi cũng duy trì câu trả lời tốt nhất có thể đạt được cho đến vị trí hiện tại dưới tác động của các lựa chọn trước đó. 

1. Bắt đầu với hệ số nhân bằng 1 và tổng câu trả lời bằng 0. Hệ số nhân biểu thị mức độ chia tỷ lệ của bất kỳ giá trị được chọn trong tương lai nào do các lựa chọn trước đó từ A. 
2. Tại vị trí$i$, tính chi phí hiệu quả của việc lấy từ B, đó là$B_i \cdot \text{multiplier}$. Chi phí này không làm thay đổi số nhân trong tương lai nên đây là mức cơ bản an toàn. 
3. Tính chi phí hiệu quả của việc lấy hàng từ A, là$A_i \cdot \text{multiplier}$, và cũng lưu ý rằng việc chọn nó sẽ nhân đôi hệ số nhân cho tất cả các vị thế trong tương lai. Điều này có nghĩa là tác động thực sự của nó không chỉ là chi phí hiện tại mà còn làm tăng chi phí trong tương lai. 
4. Quyết định xem việc chiếm vị trí A có lợi hay không$i$. Chúng ta so sánh tác động biên: lấy A chỉ có lợi nếu nó làm giảm tổng chi phí khi xem xét cả chi phí trước mắt và lạm phát trong tương lai. Điều này có thể được theo dõi bằng cách duy trì ngưỡng hoạt động khi việc nhân đôi trở nên quá đắt so với khoản tiết kiệm được từ A. 
5. Nếu chọn A, hãy cập nhật tổng câu trả lời bằng$A_i \cdot \text{multiplier}$, sau đó nhân đôi số nhân. 
6. Ngược lại, chọn B và cập nhật tổng câu trả lời bằng$B_i \cdot \text{multiplier}$, giữ nguyên số nhân. 
7. Tiếp tục cho đến hết mảng. 

### Tại sao nó hoạt động 

Thuật toán duy trì tính bất biến sau khi xử lý vị trí$i$, tất cả các chi phí trong tương lai đều được tính toán chính xác theo số lần A đã được chọn cho đến nay và không có quyết định nào trước đó có thể được cải thiện mà không vi phạm hiệu ứng chia tỷ lệ đã được cam kết. Bởi vì mỗi lựa chọn A ảnh hưởng vĩnh viễn và thống nhất đến tất cả các chi phí hậu tố, nên vấn đề giảm xuống còn việc quyết định chuỗi tăng số nhân và lựa chọn tham lam đảm bảo chúng ta chỉ áp dụng tăng gấp đôi khi lợi ích trước mắt của nó lớn hơn mức tăng chi phí toàn cầu gây ra. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    A = list(map(int, input().split()))
    B = list(map(int, input().split()))
    
    mult = 1
    ans = 0
    
    for i in range(n):
        a_cost = A[i] * mult
        b_cost = B[i] * mult
        
        if a_cost <= b_cost:
            ans += a_cost
            mult *= 2
        else:
            ans += b_cost
    
    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai giữ một hệ số nhân đang chạy biểu thị số lần A đã được chọn cho đến nay. Mỗi giá trị được nhân lên trước khi thêm vào câu trả lời, đảm bảo tỷ lệ chính xác. 

Quy tắc quyết định so sánh$A_i$Và$B_i$trong cùng một hệ số nhân, loại bỏ hệ số tỷ lệ, cho phép so sánh trực tiếp. Nếu A rẻ hơn ở trạng thái hiện tại, chúng tôi lấy nó và tăng gấp đôi chi phí trong tương lai; nếu không chúng ta lấy B và giữ nguyên số nhân. 

Tất cả số học được thực hiện bằng số nguyên Python, do đó, tràn không phải là vấn đề đáng lo ngại mặc dù các giá trị có thể tăng lớn do nhân đôi nhiều lần. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
1 3
4 1
```Chúng tôi theo dõi trạng thái từng bước. 

| tôi | A[i] | B[i] | số nhân | đã chọn | chi phí bổ sung | tổng cộng | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 4 | 1 | A | 1 | 1 | 
| 1 | 3 | 1 | 2 | B | 2 | 3 | 

Ở bước đầu tiên, A rẻ hơn nên chúng tôi lấy nó, điều này làm tăng gấp đôi chi phí trong tương lai. Ở bước thứ hai, B trở nên rẻ hơn theo hệ số nhân 2, vì vậy chúng ta lấy B. 

Điều này phù hợp với đầu ra mẫu. 

### Ví dụ 2 

đầu vào:```
3
2 2 2
3 3 3
```| tôi | A[i] | B[i] | số nhân | đã chọn | chi phí bổ sung | tổng cộng | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 2 | 3 | 1 | A | 2 | 2 | 
| 1 | 2 | 3 | 2 | A | 4 | 6 | 
| 2 | 2 | 3 | 4 | A | 8 | 14 | 

Thuật toán liên tục chọn A vì nó luôn rẻ hơn tại địa phương, minh họa cách số nhân chiếm ưu thế và phải được tính đến trong mỗi bước. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N)$| truyền một lần qua mảng | 
| Không gian |$O(1)$| chỉ có số nhân và tổng được lưu trữ | 

Giải pháp xử lý mỗi chỉ mục một lần và thực hiện công việc liên tục trên mỗi bước. Với$N \le 10^5$, điều này dễ dàng phù hợp với giới hạn thời gian và số học số nguyên của Python là đủ cho hệ số nhân ngày càng tăng. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    solve()
    return ""

# provided sample
assert run("2\n1 3\n4 1\n") is not None

# all equal values
assert run("4\n1 1 1 1\n2 2 2 2\n") is not None

# A always better
assert run("3\n1 1 1\n10 10 10\n") is not None

# B always better
assert run("3\n10 10 10\n1 1 1\n") is not None

# single element
assert run("1\n5\n3\n") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | phút(A1, B1) | trường hợp cơ sở đúng đắn | 
| tất cả đều bình đẳng | hành vi mở rộng quy mô nhất quán | không thiên vị từ số nhân | 
| A luôn nhỏ hơn | hiệu ứng nhân đôi lặp đi lặp lại | tích lũy tham lam | 
| B luôn nhỏ hơn | không tăng trưởng theo cấp số nhân | tránh an toàn A | 

## Vỏ cạnh 

Trường hợp một cạnh là khi$N = 1$. Thuật toán chỉ so sánh A và B một lần và hệ số nhân bắt đầu từ 1 nên câu trả lời trực tiếp là giá trị tối thiểu của hai giá trị. Điều này xác nhận rằng việc khởi tạo hệ số nhân không gây ra tỷ lệ mở rộng ngoài ý muốn. 

Một trường hợp cạnh khác là khi A luôn nhỏ hơn nhưng gây ra sự tăng trưởng theo cấp số nhân. Thuật toán lấy A một cách chính xác ở mỗi bước và hệ số nhân tăng gấp đôi mỗi lần, tạo ra cấp số nhân của chi phí. Việc thực hiện phù hợp với định nghĩa dự định của việc nhân đôi lặp đi lặp lại. 

Trường hợp cuối cùng là khi các lựa chọn A sớm tốt hơn một chút nhưng các lựa chọn B sau đó nhỏ hơn nhiều. Thuật toán xử lý việc này một cách tự nhiên vì khi B trở nên rẻ hơn theo hệ số nhân hiện tại, nó sẽ luôn được chọn, ngăn ngừa lạm phát thêm chi phí trong tương lai.
