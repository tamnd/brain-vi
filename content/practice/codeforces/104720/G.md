---
title: "CF 104720G - Câu đố về thực phẩm"
description: "Chúng ta được cung cấp một hệ thống câu hỏi trong đó mỗi câu hỏi được trả lời bằng cách chọn chính xác một phương án từ một tập hợp các lựa chọn cố định. Mỗi lựa chọn đều có một giá trị số và tổng kết quả bài kiểm tra chỉ là tổng của các giá trị được chọn trong tất cả các câu hỏi."
date: "2026-06-29T07:11:53+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104720
codeforces_index: "G"
codeforces_contest_name: "UTPC x WiCS Contest 10-06-23"
rating: 0
weight: 104720
solve_time_s: 69
verified: false
draft: false
---

[CF 104720G - Câu đố về thực phẩm](https://codeforces.com/problemset/problem/104720/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 9 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một hệ thống câu hỏi trong đó mỗi câu hỏi được trả lời bằng cách chọn chính xác một phương án từ một tập hợp các lựa chọn cố định. Mỗi lựa chọn đều có một giá trị số và tổng kết quả bài kiểm tra chỉ là tổng của các giá trị được chọn trong tất cả các câu hỏi. Điều quan trọng là mọi câu hỏi đều có cùng một danh sách các giá trị có thể có, do đó cấu trúc thống nhất: chúng tôi lặp lại cùng một quy trình lựa chọn một cách độc lập cho từng câu hỏi trong số n câu hỏi. 

Sau khi tổng điểm được hình thành, nó sẽ được so sánh với một số khoảng số rời rạc, mỗi khoảng tương ứng với một loại thực phẩm cụ thể. Đối với mỗi loại thực phẩm, chúng ta phải xác định xem có tồn tại ít nhất một cách trả lời câu đố sao cho tổng kết quả nằm trong khoảng của thực phẩm đó hay không. 

Do đó, nhiệm vụ tính toán cốt lõi không phải là xây dựng tất cả các câu trả lời một cách rõ ràng mà là hiểu tổng số tiền nào có thể đạt được sau n lựa chọn độc lập từ cùng một tập hợp nhiều giá trị, sau đó kiểm tra tư cách thành viên của các tổng đó trong phạm vi đã cho. 

Các ràng buộc đủ nhỏ để gợi ý mạnh mẽ một giải pháp quy hoạch động không gian trạng thái. Với n và m nhiều nhất là 20, tổng số lựa chọn tối đa là 20 bước và mỗi bước có tối đa 20 lựa chọn. Bất kỳ phép liệt kê theo cấp số nhân nào của tất cả m^n khả năng sẽ tăng lên thành 20^20, vượt xa giới hạn khả thi. Tuy nhiên, tổng tối đa có thể được giới hạn chặt chẽ: mỗi giá trị tối đa là 20, vì vậy tổng lớn nhất là 400. Điều này giúp bạn có thể theo dõi khả năng tiếp cận trên một phạm vi số nguyên nhỏ. 

Một vấn đề tế nhị có thể gây ra lời giải sai là quên rằng các chuỗi khác nhau có thể dẫn đến cùng một tổng. Việc coi các chuỗi là khác biệt là không cần thiết và tốn kém; chỉ có tập hợp các khoản tiền có thể đạt được mới quan trọng. Một sai lầm phổ biến khác là giả định các công trình xây dựng tham lam, chẳng hạn như luôn chọn giá trị tối thiểu hoặc tối đa cho mỗi câu hỏi. Điều đó không thành công vì các kết hợp trung gian có thể mở khóa những tổng mà các cực trị thuần túy không thể đạt tới. Ví dụ: với các giá trị [1, 10] và n = 2, tổng 11 có thể đạt được, nhưng chiến lược tham lam có thể bỏ lỡ những kết hợp như vậy một cách không chính xác nếu nó lý giải cục bộ cho mỗi câu hỏi. 

## Phương pháp tiếp cận 

Cách tiếp cận ngây thơ là liệt kê mọi cách có thể để trả lời câu đố. Mỗi câu trong số n câu hỏi có m lựa chọn nên tổng số phiếu trả lời hoàn chỉnh là m^n. Đối với mỗi mục, chúng tôi tính tổng và đánh dấu là có thể truy cập được. Điều này đúng vì nó xây dựng rõ ràng mọi khả năng, nhưng thời gian chạy của nó tăng theo cấp số nhân. Với cả m và n đều lên tới 20, điều này trở thành kết hợp 20^20, vượt xa mọi tính toán khả thi. 

Quan sát quan trọng là thứ tự các câu hỏi không quan trọng đối với tổng và bộ giá trị giống nhau được sử dụng lại ở mỗi bước. Điều này biến bài toán thành một cấu trúc tổng lặp lại: bắt đầu từ một tập hợp chỉ chứa 0, chúng ta liên tục thêm một lớp nữa trong đó chúng ta thêm từng giá trị trong v vào tất cả các tổng có thể đạt được trước đó. Sau n lớp, chúng ta thu được tất cả các tổng có thể đạt được. Vì tổng tối đa chỉ là 400 nên chúng ta có thể duy trì DP boolean một cách an toàn trên tổng và lặp lại n lần. 

Điều này làm giảm vấn đề từ việc liệt kê theo cấp số nhân trên các chuỗi đến việc truyền bá theo thời gian đa thức trên một không gian trạng thái giới hạn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(m^n · n) | O(1) | Quá chậm | 
| DP tối ưu | O(n · m · S) trong đó S ≤ 400 | O(S) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

## Xây dựng DP tối ưu

1. Khởi tạo một mảng boolean dp trong đó dp[s] cho biết liệu có thể đạt được tổng sau khi xử lý một số câu hỏi hay không. Đặt dp[0] = true vì trước khi trả lời bất kỳ câu hỏi nào, tổng bằng 0. 
2. Lặp lại quy trình sau chính xác n lần, một lần cho mỗi câu hỏi. Mỗi lần lặp đại diện cho việc thêm một giá trị được chọn vào tổng số tiền. 
3. Đối với mỗi lần lặp, hãy tạo một mảng mới next_dp được khởi tạo thành false. Mảng này sẽ lưu trữ tất cả số tiền có thể truy cập được sau khi trả lời thêm một câu hỏi. 
4. Với mọi tổng s sao cho dp[s] đúng, hãy thử mở rộng nó với mọi giá trị có thể có v_i. Đánh dấu next_dp[s + v_i] là đúng. Điều này tương ứng với việc chọn câu trả lời v_i cho câu hỏi hiện tại. 
5. Sau khi xử lý tất cả các khoản tiền và giá trị, hãy thay thế dp bằng next_dp. Điều này đảm bảo rằng mỗi lớp chỉ sử dụng kết quả từ số lượng câu hỏi trước đó, tránh việc vô tình sử dụng lại trong cùng một bước. 
6. Sau khi hoàn thành tất cả n lần lặp, dp sẽ mã hóa tất cả các điểm cuối cùng có thể có. 
7. Đối với mỗi khoảng thực phẩm [l, r], hãy kiểm tra xem có tồn tại bất kỳ s nào trong phạm vi này sao cho dp[s] là đúng hay không. Nếu số tiền đó tồn tại thì câu trả lời là CÓ; nếu không thì KHÔNG. 

Lý do chúng ta có thể quét từng khoảng một cách an toàn là vì mỗi quyết định về thực phẩm chỉ phụ thuộc vào sự tồn tại của ít nhất một tổng hợp lệ chứ không phụ thuộc vào số lượng tổng như vậy tồn tại hay chúng trùng lặp với các khoảng khác như thế nào. 

### Tại sao nó hoạt động 

DP duy trì tính bất biến rằng sau i lần lặp, dp thể hiện chính xác tất cả các tổng có thể đạt được bằng cách sử dụng i câu hỏi lựa chọn. Mỗi chuyển đổi thêm chính xác một lựa chọn từ tập hợp các giá trị được phép, duy trì tính chính xác vì mọi chuỗi hợp lệ có độ dài i + 1 đều có thể được phân tách thành một chuỗi hợp lệ có độ dài i cộng với một lựa chọn cuối cùng. Ngược lại, mọi chuyển đổi được xây dựng đều tương ứng với một chuỗi lựa chọn thực sự. Sự kết hợp giữa các chuyển đổi và chuỗi câu trả lời hợp lệ này đảm bảo tính đầy đủ và đúng đắn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    v = list(map(int, input().split()))
    q = int(input())
    intervals = [tuple(map(int, input().split())) for _ in range(q)]

    max_sum = n * max(v)
    dp = [False] * (max_sum + 1)
    dp[0] = True

    for _ in range(n):
        ndp = [False] * (max_sum + 1)
        for s in range(max_sum + 1):
            if not dp[s]:
                continue
            for val in v:
                if s + val <= max_sum:
                    ndp[s + val] = True
        dp = ndp

    for l, r in intervals:
        ok = False
        for s in range(l, r + 1):
            if 0 <= s <= max_sum and dp[s]:
                ok = True
                break
        print("YES" if ok else "NO")

if __name__ == "__main__":
    solve()
```Việc triển khai phản ánh trực tiếp việc xây dựng DP. Mảng dp được phân bổ lại ở mỗi bước để ngăn trạng thái trộn lẫn từ các số lượng câu hỏi khác nhau. Việc kiểm tra ranh giới đảm bảo chúng tôi không bao giờ lập chỉ mục vượt quá tổng tối đa có thể n · max(v). Việc kiểm tra khoảng thời gian được thực hiện bằng cách quét đơn giản vì tổng phạm vi đủ nhỏ nên việc truyền tải toàn bộ cũng không đáng kể. 

Một cạm bẫy phổ biến là cố gắng cập nhật dp tại chỗ. Điều đó sẽ cho phép sử dụng nhiều lần cùng một câu hỏi một cách không chính xác
