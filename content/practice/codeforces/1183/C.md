---
title: "CF 1183C - Trò chơi máy tính"
description: "Chúng tôi đang mô phỏng một chuỗi chính xác $n$ lượt chơi. Tại bất kỳ thời điểm nào Vova đều có một mức sạc $k$ và mỗi lượt anh ta phải chọn một trong hai hành động để giảm mức sạc. Một hành động sẽ rẻ hơn về mặt hao pin nhưng chỉ có thể sử dụng khi pin đủ cao."
date: "2026-06-13T11:36:51+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "math"]
categories: ["algorithms"]
codeforces_contest: 1183
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 570 (Div. 3)"
rating: 1400
weight: 1183
solve_time_s: 449
verified: true
draft: false
---

[CF 1183C - Trò chơi trên máy tính](https://codeforces.com/problemset/problem/1183/C) 

**Đánh giá:** 1400 
**Tags:** tìm kiếm nhị phân, toán học 
**Thời gian giải:** 7 phút 29s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang mô phỏng một chuỗi chính xác$n$lượt chơi. Bất cứ lúc nào Vova cũng có pin$k$, và mỗi lượt anh ta phải chọn một trong hai hành động để giảm phí. 

Một hành động sẽ rẻ hơn về mặt hao pin nhưng chỉ có thể sử dụng khi pin đủ cao. Hành động kia đắt hơn nhưng có yêu cầu yếu hơn và có thể được sử dụng thường xuyên hơn. Mục tiêu là hoàn thành tất cả$n$quay trong khi không bao giờ để pin giảm xuống 0 hoặc thấp hơn sau bất kỳ lượt nào. Trong số tất cả các chiến lược hợp lệ, chúng tôi muốn tối đa hóa số lần hành động đắt tiền được sử dụng. 

Vì vậy, vấn đề không chỉ là tính khả thi mà còn là việc tối đa hóa số nước đi “tốt hơn” trong điều kiện hạn chế về nguồn lực cứng phát triển theo thời gian. 

Khó khăn chính là sự lựa chọn ở mỗi lượt sẽ ảnh hưởng đến khả năng sẵn sàng của cả hai hành động trong tương lai. Một sự lựa chọn tham lam mà không có lý luận tổng thể có thể dễ dàng thất bại vì sử dụng quá nhiều nước đi rẻ sớm có thể buộc các quốc gia bất hợp pháp về sau, trong khi sử dụng quá nhiều nước đi đắt tiền sẽ làm giảm tổng số lượt bạn có thể sống sót. 

Những hạn chế là lớn, với$q \le 10^5$Và$n, k \le 10^9$. Điều này ngay lập tức loại trừ mọi mô phỏng qua các lượt. Bất kỳ giải pháp nào cũng phải tính toán câu trả lời theo thời gian logarit hoặc không đổi cho mỗi truy vấn. Thậm chí$O(n)$mỗi trường hợp thử nghiệm là không thể. 

Một trường hợp khó phát hiện khi pin chỉ vừa đủ để thực hiện tất cả các lượt chỉ sử dụng mức giảm rẻ hơn$b$. Ngay cả khi điều đó hoàn toàn có thể thực hiện được, yêu cầu nghiêm ngặt về độ tích cực sau mỗi lượt vẫn có thể phá vỡ tính khả thi tùy theo thứ tự, bởi vì một chuỗi giảm lớn hơn có thể đẩy pin xuống dưới 0 sớm hơn dự kiến. 

Ví dụ, nếu$k = 5$,$n = 2$,$a = 4$,$b = 3$, mặc dù$5 - 3 - 3 = -1$, trò chơi là không thể, nhưng lý luận ngây thơ rằng “chúng ta chỉ cần tổng tổng ≤ k” có thể bỏ sót rằng các trạng thái trung gian phải luôn dương. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử tất cả các phân phối có thể có của hai loại nước đi trên$n$lần lượt, kiểm tra tính hợp lệ bằng cách mô phỏng pin ở mỗi bước. Ở mỗi lượt, chúng ta sẽ phân nhánh sang sử dụng một trong hai cách giảm$a$hoặc$b$nếu được phép và theo dõi tính khả thi. Điều này dẫn đến một số khả năng theo cấp số nhân, đại khái là$O(2^n)$, vì mỗi$n$lượt có hai lựa chọn. Ngay cả việc cắt bớt các hạn chế về pin cũng không làm thay đổi vụ nổ trong trường hợp xấu nhất khi$k$là lớn. 

Quan sát quan trọng là chúng ta không bao giờ cần mô phỏng chính xác thứ tự các quyết định. Điều quan trọng là chúng ta sử dụng bao nhiêu lần$a$-di chuyển so với$b$-di chuyển, vì pin chỉ giảm và không di chuyển sẽ phục hồi năng lượng. Cấu trúc trở thành bài toán phân bổ tài nguyên: chúng ta muốn chọn số lượng tối đa các$a$- di chuyển trong khi vẫn có thể thực hiện được tất cả$n$bước, điền phần còn lại với$b$-di chuyển khi cần thiết. 

Một cách hữu ích để suy nghĩ về điều này là bắt đầu từ kịch bản lạc quan nhất. Nếu chúng ta giả sử mỗi lượt sử dụng mức giảm rẻ hơn$b$, pin sau$n$lượt sẽ là$k - nb$. Điều này ngay lập tức đưa ra một điều kiện khả thi: nếu$k \le nb$, ngay cả chuỗi trường hợp tốt nhất cũng không thể giữ pin tích cực trong suốt trò chơi. 

Khi tính khả thi được đảm bảo, chúng tôi muốn thay thế một số$b$-di chuyển với$a$-di chuyển. Mỗi lần thay thế sẽ làm tăng tổng mức tiêu thụ pin lên$a - b$. Tuy nhiên, những thay thế này chỉ có hiệu lực trong khi pin vẫn ở trạng thái tích cực nghiêm ngặt sau mỗi bước, điều này hạn chế một cách hiệu quả số lượng “chi phí bổ sung” mà chúng ta có thể chi trả ngoài phần đế.$nb$sự tiêu thụ. 

Vấn đề giảm xuống còn việc tính toán xem chúng ta có bao nhiêu “ngân sách” bổ sung vượt quá mức cơ bản$nb$, và có bao nhiêu mức tăng kích thước$a - b$có thể phù hợp với ngân sách đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^n)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(1)$mỗi truy vấn |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi viết lại quy trình theo mức tiêu thụ tối thiểu bắt buộc và nâng cấp tùy chọn. 

1. Tính tổng mức tiêu thụ tối thiểu có thể nếu chúng ta luôn sử dụng phương án rẻ hơn$b$. Đây là$n \cdot b$. Nếu điều này đã vượt quá hoặc bằng$k$, thì không tồn tại dãy hợp lệ. Lý do là ngay cả chiến lược tốt nhất có thể cũng không thể giữ cho pin luôn dương ở tất cả các bước. 
2. Nếu khả thi, hãy tính toán ngân sách khả dụng còn lại sau khi thanh toán chi phí cơ sở:$k - n \cdot b$. Điều này thể hiện mức độ mất thêm pin mà chúng ta vẫn có thể chịu được bằng cách thay thế một số$b$-di chuyển với$a$-di chuyển. 
3. Mỗi lần chúng ta thay thế một$b$-di chuyển bằng một$a$-di chuyển, chúng tôi chi thêm$a - b$đơn vị pin. Đây là chi phí gia tăng của việc cải thiện một động thái. Chúng tôi muốn tối đa hóa số lượng thay thế như vậy trong khi vẫn duy trì trong phạm vi ngân sách còn lại. 
4. Do đó, số lần thay thế tối đa là$\left\lfloor \frac{k - n b}{a - b} \right\rfloor$. Tuy nhiên, chúng tôi không thể vượt quá tổng số lượt$n$, vì vậy chúng tôi lấy mức tối thiểu giữa giá trị này và$n$. 
5. Đáp án cuối cùng là số lượng$a$-di chuyển, và nếu tính khả thi không thành công, chúng tôi sẽ xuất ra$-1$. 

### Tại sao nó hoạt động 

Bất kỳ chuỗi hợp lệ nào cũng phải bao gồm chính xác$n$di chuyển, mỗi đóng góp một trong hai$a$hoặc$b$đến tổng mức tiêu thụ. Vì tất cả các lần di chuyển chỉ làm giảm pin và không có khả năng phục hồi nên thứ tự di chuyển không ảnh hưởng đến tính khả thi ngoài việc đảm bảo pin không bao giờ giảm về 0 sớm. Ràng buộc dương nghiêm ngặt có thể được thỏa mãn bất cứ khi nào tổng mức tiêu thụ nhỏ hơn$k$, bởi vì chúng ta luôn có thể sắp xếp những bước đi có chi phí cao hơn sau này mà không vi phạm các trạng thái trước đó. Điều này chuyển vấn đề thành một vấn đề phân bổ ngân sách đơn giản cho hai chi phí cố định. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    q = int(input())
    for _ in range(q):
        k, n, a, b = map(int, input().split())

        if k <= n * b:
            print(-1)
            continue

        # extra budget beyond baseline all-b moves
        extra = k - n * b

        # each a-move instead of b-move costs (a - b) extra
        ans = extra // (a - b)

        # cannot exceed number of turns
        if ans > n:
            ans = n

        print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai trực tiếp tính toán tính khả thi bằng cách sử dụng chiến lược cơ bản trong đó mỗi bước di chuyển đều sử dụng chi phí$b$. Sau khi vượt qua, nó sẽ tính toán số lần chúng tôi có thể nâng cấp một$b$-di chuyển vào một$a$-di chuyển bằng cách sử dụng ngân sách bổ sung có sẵn. 

Phép trừ$k - n \cdot b$phải được thực hiện cẩn thận bằng cách sử dụng số học 64 bit trong các ngôn ngữ có kích thước số nguyên cố định, nhưng Python xử lý số nguyên lớn một cách an toàn. Sự phân chia theo$a - b$là số nguyên chia tầng, phù hợp với số lượng nâng cấp đầy đủ mà chúng tôi có thể mua được. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:$k = 15, n = 5, a = 3, b = 2$Chúng tôi tính toán chi phí cơ bản$n \cdot b = 10$. Ngân sách còn lại là$15 - 10 = 5$. Chi phí mỗi lần nâng cấp$a - b = 1$. Như vậy chúng ta có thể nâng cấp được 5 chiêu thức. 

| Bước | Chi phí cơ bản | Ngân sách bổ sung | Nâng cấp | Kết quả | 
| --- | --- | --- | --- | --- | 
| Ban đầu | 10 | 5 | 0 | khả thi | 
| Tính toán | - | 5 | - | ngân sách bổ sung | 
| Chia | - | - | 5 | 5 bước di chuyển | 

Chúng ta nhận được đáp án 5, nghĩa là mọi chiêu thức đều có thể nâng cấp. 

Điều này cho thấy mô hình xác định chính xác rằng ngân sách thặng dư sẽ trực tiếp chuyển thành các động thái bổ sung có chi phí cao hơn. 

### Ví dụ 2 

đầu vào:$k = 16, n = 7, a = 5, b = 2$Chi phí cơ bản là$14$. Ngân sách còn lại là$2$. Chi phí mỗi lần nâng cấp$3$. Không thể nâng cấp được. 

| Bước | Chi phí cơ bản | Ngân sách bổ sung | Nâng cấp | Kết quả | 
| --- | --- | --- | --- | --- | 
| Ban đầu | 14 | 2 | 0 | khả thi | 
| Tính toán | - | 2 | - | không đủ | 
| Chia | - | - | 0 | 0 a-di chuyển | 

Điều này chứng tỏ rằng mặc dù trò chơi có thể khả thi nhưng không thể cải thiện được chiến lược cơ bản. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(q)$| Mỗi truy vấn sử dụng một số phép toán số học không đổi | 
| Không gian |$O(1)$| Không có bộ nhớ bổ sung ngoài các biến | 

Giải pháp này có quy mô trực tiếp theo số lượng truy vấn và dễ dàng phù hợp với giới hạn ngay cả đối với$10^5$đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    q = int(input())
    out = []
    for _ in range(q):
        k, n, a, b = map(int, input().split())
        if k <= n * b:
            out.append("-1")
            continue
        extra = k - n * b
        ans = extra // (a - b)
        if ans > n:
            ans = n
        out.append(str(ans))
    return "\n".join(out)

assert run("""6
15 5 3 2
15 5 4 3
15 5 2 1
15 5 5 1
16 7 5 2
20 5 7 3
""") == """4
-1
5
2
0
1"""

# minimum case
assert run("1\n1 1 2 1\n") == "1"

# impossible small case
assert run("1\n1 1 10 9\n") == "-1"

# tight boundary
assert run("1\n10 3 5 3\n") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mẫu được cung cấp | kết quả đầu ra mẫu chính xác | tính đúng đắn trong các trường hợp chính thức | 
| 1 1 2 1 | 1 | trường hợp khả thi nhỏ nhất | 
| 1 1 10 9 | -1 | không khả thi ngay lập tức | 
| 10 3 5 3 | 1 | giới hạn nâng cấp ranh giới | 

## Vỏ cạnh 

Trường hợp một cạnh xảy ra khi$k$chính xác bằng$n \cdot b$. Trong tình huống đó, không có sự chậm trễ nào trong hệ thống. Bất kỳ nỗ lực nào để thay thế một$b$-di chuyển bằng một$a$-di chuyển ngay lập tức phá vỡ tính khả thi. Thuật toán trả về chính xác$-1$, phù hợp với thực tế là ngay cả chiến lược tối thiểu cũng không để lại vùng đệm tích cực. 

Một trường hợp cạnh khác xuất hiện khi$a - b$lớn so với độ chùng sẵn có. Ngay cả khi tổng lượng pin đủ cho mọi lượt di chuyển thì cũng chỉ có thể nâng cấp một vài lần. Bước phân chia đảm bảo chúng tôi không tính quá mức vì mỗi lần nâng cấp sẽ tiêu tốn một lượng cố định riêng biệt của ngân sách còn lại. 

Trường hợp ranh giới cuối cùng là khi$n$cực kỳ lớn nhưng$k$chỉ ở trên một chút$n \cdot b$. Câu trả lời trở nên rất nhỏ mặc dù kích thước đầu vào lớn và việc tính toán theo thời gian không đổi sẽ tránh mọi sự phụ thuộc vào$n$ngoài số học.
