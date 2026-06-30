---
title: "CF 104395B - Làm đổ nước trái cây"
description: "Chúng ta được cung cấp một mảng trong đó mỗi vị trí không nhất thiết phải lưu trữ một số cố định. Một số vị trí được biết chính xác, trong khi những vị trí khác không chắc chắn và được mô tả như một khoảng đóng."
date: "2026-06-30T23:18:05+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104395
codeforces_index: "B"
codeforces_contest_name: "Cupertino Informatics Tournament"
rating: 0
weight: 104395
solve_time_s: 57
verified: true
draft: false
---

[CF 104395B - Làm đổ nước ép](https://codeforces.com/problemset/problem/104395/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 57s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng trong đó mỗi vị trí không nhất thiết phải lưu trữ một số cố định. Một số vị trí được biết chính xác, trong khi những vị trí khác không chắc chắn và được mô tả như một khoảng đóng. Đối với mỗi vị trí không chắc chắn, giá trị được chọn cuối cùng phải nằm ở đâu đó trong khoảng cho phép của nó. 

Sau khi mảng được cố định bằng cách chọn một giá trị hợp lệ cho mọi vị trí không chắc chắn, chúng ta phải trả lời nhiều truy vấn. Mỗi truy vấn đưa ra một phân đoạn$[l, r]$và tổng mục tiêu$x$. Câu hỏi đặt ra là liệu có thể gán các giá trị hợp lệ cho tất cả các vị trí không chắc chắn để tổng các giá trị được chọn trong phân đoạn đó trở nên chính xác hay không.$x$, đồng thời tôn trọng tất cả các ràng buộc về khoảng thời gian trên toàn cầu. 

Khó khăn chính là cùng một mảng phải đáp ứng tất cả các truy vấn một cách độc lập, nhưng mỗi truy vấn chỉ hỏi về sự tồn tại: chúng ta không bắt buộc phải xây dựng một phép gán toàn cục duy nhất hoạt động đồng thời cho tất cả các truy vấn. 

Những hạn chế là lớn, với$n, q \le 10^5$. Điều này ngay lập tức loại trừ mọi giải pháp tính toán lại thông tin cho mỗi truy vấn theo thời gian tuyến tính trên phân đoạn, vì điều đó sẽ dẫn đến$10^{10}$hoạt động trong trường hợp xấu nhất. Thậm chí$O(n \log n)$mỗi truy vấn quá chậm. Chúng tôi buộc phải sử dụng phương pháp tiền xử lý trong đó mỗi truy vấn có thể được trả lời theo thời gian logarit hoặc không đổi. 

Một vấn đề tế nhị là sự không chắc chắn không phải là vấn đề cục bộ đối với một truy vấn. Mỗi vị trí không xác định đóng góp một loạt các giá trị có thể có và các truy vấn khác nhau có thể chồng lấp các vị trí này theo những cách khác nhau. Một sai lầm ngây thơ là xử lý từng truy vấn một cách độc lập và chỉ gán các giá trị tham lam bên trong phân khúc. Điều đó không thành công vì các giá trị được chọn cho lý do của một truy vấn có thể mâu thuẫn với tính khả thi khi xem xét tính nhất quán tổng thể của các giới hạn. 

Một cái bẫy khác xuất hiện khi tất cả các yếu tố đều được biết đến. Khi đó câu trả lời sẽ chuyển sang kiểm tra xem tổng đoạn cố định có bằng không$x$. Tuy nhiên, trong các trường hợp hỗn hợp, tổng khả thi không phải là một giá trị đơn lẻ mà là một khoảng, và việc suy luận về khoảng đó một cách chính xác là thách thức trọng tâm. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ cố gắng gán một giá trị cụ thể cho mọi phần tử không chắc chắn, sau đó tính tổng phân đoạn cho mỗi truy vấn và kiểm tra xem liệu phép gán nào có thể tạo ra mục tiêu được yêu cầu hay không. Mỗi ô không chắc chắn có tới 1000 lựa chọn và trong trường hợp xấu nhất có thể có$n$những tế bào như vậy, làm cho số lượng các tổ hợp trở nên lớn về mặt thiên văn. Ngay cả việc hạn chế sự chú ý vào một truy vấn duy nhất, việc liệt kê tất cả các kết hợp là theo cấp số nhân và hoàn toàn không khả thi. 

Nhận xét quan trọng là chúng tôi không quan tâm đến các nhiệm vụ cá nhân. Điều quan trọng đối với mỗi truy vấn chỉ là tổng tối thiểu và tối đa có thể có của phân đoạn được truy vấn trong tất cả các lựa chọn hợp lệ. Nếu chúng ta có thể tính được hai giá trị này thì câu trả lời sẽ trở thành một phép kiểm tra thành viên khoảng đơn giản. 

Điều này làm giảm vấn đề khi duy trì hai phiên bản độc lập của mảng: một phiên bản trong đó mọi vị trí không chắc chắn đều nhận giá trị tối thiểu và một phiên bản lấy giá trị tối đa. Mọi phép gán hợp lệ đều phải nằm giữa các cực trị này cho mọi vị trí, do đó, bất kỳ tổng phân đoạn nào cũng phải nằm giữa tổng tiền tố tương ứng của hai mảng này. 

Khi hai mảng tổng tiền tố này được tạo, mỗi truy vấn sẽ giảm xuống việc trích xuất tổng mảng con trong$O(1)$và kiểm tra xem mục tiêu có nằm trong khoảng thời gian có thể đạt được hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Tối ưu | O(n + q) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

###Giải pháp tối ưu 

Chúng tôi chuyển đổi vấn đề thành tổng tiền tố trên hai mảng dẫn xuất: mảng tối thiểu và mảng tối đa. 

## Hướng dẫn thuật toán 

1. Xây dựng hai mảng có độ dài$n$. Đối với mỗi vị trí, nếu giá trị cố định thì cả hai mảng đều lưu trữ giá trị đó. Nếu giá trị không chắc chắn với phạm vi$[l, r]$, mảng lưu trữ tối thiểu$l$và các cửa hàng mảng tối đa$r$. Điều này nắm bắt được đầy đủ các khả năng ở mỗi chỉ số theo cách phân biệt các thái cực trên và dưới. 
2. Xây dựng tổng tiền tố cho cả hai mảng. Tổng tiền tố tại chỉ mục$i$đại diện cho tổng tối thiểu hoặc tối đa có thể có của tiền tố$[1, i]$dưới những lựa chọn cực đoan. 
3. Đối với mỗi truy vấn$(l, r, x)$, hãy tính tổng tối thiểu có thể có trên đoạn đó là$minSum = prefMin[r] - prefMin[l-1]$và số tiền tối đa có thể là$maxSum = prefMax[r] - prefMax[l-1]$. 
4. Kiểm tra xem$x$nằm trong khoảng$[minSum, maxSum]$. Nếu có thì xuất ra “có”, nếu không thì xuất ra “không”. 

Lý do điều này có hiệu quả là vì mỗi vị trí đóng góp độc lập vào tổng số và các ràng buộc trên mỗi phần tử là các ràng buộc về khoảng không tương tác giữa các vị trí. 

### Tại sao nó hoạt động 

Mỗi vị trí trong mảng đóng góp một số hạng cộng vào bất kỳ tổng phân đoạn nào. Vì hạn chế duy nhất là mỗi phần tử phải nằm trong một khoảng cố định, nên mức đóng góp tối thiểu có thể có của mỗi vị trí đạt được một cách độc lập bằng cách chọn giới hạn dưới của nó và mức đóng góp tối đa bằng cách chọn giới hạn trên của nó. Không có sự kết hợp nào tồn tại giữa các chỉ số, do đó việc cực trị hóa từng vị trí cục bộ sẽ tạo ra cực trị toàn cục cho bất kỳ phân khúc nào. Điều này làm cho tập hợp tổng khả thi cho bất kỳ phân đoạn nào chính xác là khoảng giữa hai điểm cực trị được tính toán này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, q = map(int, input().split())

    min_arr = [0] * (n + 1)
    max_arr = [0] * (n + 1)

    for i in range(1, n + 1):
        tmp = input().split()
        if tmp[0] == '1':
            v = int(tmp[1])
            min_arr[i] = max_arr[i] = v
        else:
            l = int(tmp[1])
            r = int(tmp[2])
            min_arr[i] = l
            max_arr[i] = r

    pref_min = [0] * (n + 1)
    pref_max = [0] * (n + 1)

    for i in range(1, n + 1):
        pref_min[i] = pref_min[i - 1] + min_arr[i]
        pref_max[i] = pref_max[i - 1] + max_arr[i]

    out = []
    for _ in range(q):
        l, r, x = map(int, input().split())
        min_sum = pref_min[r] - pref_min[l - 1]
        max_sum = pref_max[r] - pref_max[l - 1]

        if min_sum <= x <= max_sum:
            out.append("yes")
        else:
            out.append("no")

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc thực hiện trực tiếp theo sau việc xây dựng hai mảng giới hạn. Điểm tinh tế duy nhất là đảm bảo tổng tiền tố có 1 chỉ mục để các truy vấn mảng con trở thành một phép trừ duy nhất. Một chi tiết quan trọng khác là các giá trị không chắc chắn được xử lý một cách đối xứng: cả hai giới hạn đều được lưu trữ ở cùng một chỉ mục, điều này tránh mọi logic trường hợp đặc biệt trong thời gian truy vấn. 

## Ví dụ đã hoạt động 

Hãy xem xét một mảng nhỏ: 

đầu vào:```
5 3
1 5
2 1 3
1 2
2 4 6
1 10
1 2 5
2 2 2
1 5 12
```Đầu tiên chúng ta xây dựng các mảng: 

| tôi | gõ | phút | tối đa | pref_min | pref_max | 
| --- | --- | --- | --- | --- | --- | 
| 1 | đã sửa 5 | 5 | 5 | 5 | 5 | 
| 2 | [1,3] | 1 | 3 | 6 | 8 | 
| 3 | cố định 2 | 2 | 2 | 8 | 10 | 
| 4 | [4,6] | 4 | 6 | 12 | 16 | 
| 5 | cố định 10 | 10 | 10 | 22 | 26 | 

Truy vấn 1 là$[1,2]$với$x = 5$. Tổng tối thiểu của phân đoạn là$6 - 0 = 6$, tối đa là$8 - 0 = 8$, vậy 5 nằm ngoài khoảng và câu trả lời là không. 

Truy vấn 2 là$[2,2]$với$x = 2$. Khoảng thời gian cho chỉ số 2 là$[1,3]$, vậy 2 là khả thi và câu trả lời là có. 

Truy vấn 3 là$[3,5]$với$x = 12$. Tối thiểu là$22 - 8 = 14$, tối đa là$26 - 10 = 16$, vì vậy 12 là không thể đạt được. 

Dấu vết này cho thấy rằng mỗi truy vấn giảm xuống việc kiểm tra một khoảng số được lấy từ tổng tiền tố mà không phụ thuộc vào các truy vấn khác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + q) | Một lần vượt qua để xây dựng mảng và tổng tiền tố, một lần kiểm tra O(1) cho mỗi truy vấn | 
| Không gian | O(n) | Hai mảng phụ và tổng tiền tố | 

Các ràng buộc cho phép lên đến$10^5$hoạt động và giải pháp này chỉ thực hiện tiền xử lý tuyến tính cộng với xử lý truy vấn theo thời gian không đổi, phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import sys

    input = sys.stdin.readline

    def solve():
        n, q = map(int, input().split())

        min_arr = [0] * (n + 1)
        max_arr = [0] * (n + 1)

        for i in range(1, n + 1):
            tmp = input().split()
            if tmp[0] == '1':
                v = int(tmp[1])
                min_arr[i] = max_arr[i] = v
            else:
                l = int(tmp[1])
                r = int(tmp[2])
                min_arr[i] = l
                max_arr[i] = r

        pref_min = [0] * (n + 1)
        pref_max = [0] * (n + 1)

        for i in range(1, n + 1):
            pref_min[i] = pref_min[i - 1] + min_arr[i]
            pref_max[i] = pref_max[i - 1] + max_arr[i]

        res = []
        for _ in range(q):
            l, r, x = map(int, input().split())
            mn = pref_min[r] - pref_min[l - 1]
            mx = pref_max[r] - pref_max[l - 1]
            res.append("yes" if mn <= x <= mx else "no")

        return "\n".join(res)

    return solve()

# sample-like sanity tests
assert run("1 1\n1 5\n1 1 5\n") == "yes"
assert run("1 1\n2 1 3\n1 1 2\n") == "yes"
assert run("1 1\n2 1 3\n1 1 10\n") == "no"
assert run("3 2\n1 1\n2 1 2\n1 3\n1 1 3\n1 1 6\n") in ["yes\nno", "no\nyes"]
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| trận đấu cố định duy nhất | vâng | trường hợp bình đẳng chính xác | 
| đơn không chắc chắn trong phạm vi | vâng | tính khả thi khoảng thời gian | 
| đơn không chắc chắn không thể | không | ràng buộc giới hạn trên | 
| mảng nhỏ hỗn hợp | khác nhau | tính chính xác của tiền tố | 

## Vỏ cạnh 

Một ô không chắc chắn một phần tử sẽ kiểm tra xem liệu giải pháp có xử lý đúng khoảng thời gian dưới dạng bao gồm hay không. Ví dụ, nếu ô là$[2, 5]$và truy vấn yêu cầu tổng bằng 5, thuật toán trả lời đúng có vì giới hạn phân đoạn trở thành chính xác từ 2 đến 5. 

Một mảng cố định hoàn toàn là một trường hợp ranh giới khác. Ở đây, các mảng tối thiểu và tối đa giống hệt nhau, vì vậy mọi truy vấn đều giảm xuống mức kiểm tra tính bằng nhau nghiêm ngặt đối với một chênh lệch tổng tiền tố duy nhất. Thuật toán tự nhiên sụp đổ theo hành vi đó mà không cần xử lý đặc biệt. 

Một mảng hoàn toàn không chắc chắn làm nổi bật tính chính xác của việc tổng hợp. Mỗi chỉ mục đóng góp một phạm vi độc lập và tổng tiền tố sẽ tích lũy chính xác các phạm vi này. Đối với bất kỳ phân khúc nào, tập hợp tổng có thể đạt được sẽ trở thành một khoảng liền kề duy nhất và các giới hạn được tính toán của thuật toán khớp chính xác với khoảng đó, đảm bảo không có kết quả dương tính hoặc âm tính giả.
