---
title: "CF 105257C - Ghế"
description: "Chúng ta có $n$ người và sắp xếp chỗ ngồi ban đầu cố định trong đó người $i$ bắt đầu ở ghế $i$. Mỗi chỗ ngồi là duy nhất và mỗi người chiếm đúng một chỗ. Mỗi người cũng có một chỗ ngồi ưa thích $ai$, nằm ở đâu đó trong nhóm chỗ ngồi lớn hơn $1 ldots 2n$."
date: "2026-06-24T05:01:33+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105257
codeforces_index: "C"
codeforces_contest_name: "2024 ICPC ShaanXi Provincial Contest"
rating: 0
weight: 105257
solve_time_s: 70
verified: true
draft: false
---

[CF 105257C - Ghế](https://codeforces.com/problemset/problem/105257/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 10s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được trao$n$người và sự sắp xếp chỗ ngồi cố định ban đầu nơi người$i$bắt đầu tại chỗ ngồi$i$. Mỗi chỗ ngồi là duy nhất và mỗi người chiếm đúng một chỗ. 

Mỗi người cũng có một chỗ ngồi ưa thích$a_i$, nằm ở đâu đó trong một nhóm chỗ ngồi lớn hơn$1 \ldots 2n$. Đối với mỗi người, chúng tôi được phép giữ họ ở chỗ ngồi ban đầu$i$, hoặc di chuyển họ đến chỗ ngồi ưa thích của họ$a_i$. Không có hai người nào có thể ngồi cùng một chỗ trong sự sắp xếp cuối cùng. 

Mục tiêu là tối đa hóa số lượng người ngồi vào chỗ ngồi ưa thích của họ. 

Đầu ra chỉ là số lượng người tối đa có thể được bố trí thành công vào chỗ ngồi ưa thích của họ trong điều kiện ràng buộc này. 

Những ràng buộc cho phép$n$lên đến$10^5$, do đó, bất kỳ giải pháp nào thử tất cả các tập hợp con hoặc mô phỏng các bài tập có giải pháp xung đột lặp đi lặp lại sẽ quá chậm. Bất cứ điều gì bậc hai hoặc liên quan đến việc truyền đồ thị lặp đi lặp lại cho mỗi lựa chọn ứng cử viên sẽ không vượt qua. 

Một khó khăn nhỏ là việc chọn một người ngồi vào chỗ ngồi ưa thích của họ có thể buộc những người khác phải rời khỏi chỗ mà lẽ ra họ sẽ giữ. Ví dụ, nếu hai người muốn ngồi chung một chỗ thì chỉ một người được ngồi, còn người còn lại phải lùi về chỗ ngồi ban đầu. Tuy nhiên, bản thân dự phòng đó có thể bị chặn nếu người khác đã xác nhận đó là chỗ ngồi ưu tiên. 

Các trường hợp cạnh chính đến từ va chạm: 

Ví dụ: nếu nhiều người muốn có cùng một chỗ ngồi thì chỉ một người có thể được hưởng lợi từ chỗ đó$a = [5, 5, 5]$chỉ cho phép một thành công duy nhất cho ghế số 5. 

Nếu sở thích hình thành chuỗi như$1 \to 2 \to 3 \to 1$, tất cả chúng vẫn có thể được thỏa mãn đồng thời vì chúng hoán vị với nhau. 

Hai hành vi này gợi ý rằng xung đột chỉ quan trọng khi nhiều cạnh nhắm vào cùng một chỗ ngồi chứ không phải khi các phần phụ thuộc hình thành theo chu kỳ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng quyết định, đối với mỗi người, liệu có nên giữ họ ở mức không?$i$hoặc di chuyển chúng đến$a_i$, đồng thời đảm bảo không có ghế nào được sử dụng hai lần. Về cơ bản, đây là tìm kiếm trên tất cả các tập hợp con hợp lệ của các nước đi đã chọn. Không gian trạng thái có tính hàm mũ và thậm chí một mô phỏng tham lam sẽ liên tục giải quyết các xung đột lan truyền thông qua cấu trúc phụ thuộc chỗ ngồi, dẫn đến hành vi bậc hai tiềm ẩn khi xảy ra các tầng. 

Sự đơn giản hóa chính xuất phát từ việc quan sát xung đột thực sự phát sinh như thế nào. Mỗi người đóng góp đúng một ghế mục tiêu ứng cử viên$a_i$. Hạn chế thực sự duy nhất đối với việc lựa chọn những động thái này là một chỗ ngồi không thể được chỉ định cho nhiều người chọn phương án ưa thích của họ. 

Nếu nhiều người chỉ vào cùng một chỗ thì nhiều nhất một người trong số họ có thể thành công. Nếu chúng ta chọn một trong số đó, chiếc ghế đó sẽ có người ngồi và buộc chủ sở hữu ban đầu của nó phải di chuyển, nhưng điều đó không làm giảm số lượng người hài lòng mà chúng ta có thể đạt được ở nơi khác. 

Điều này có nghĩa là mỗi chỗ ngồi$v$có thể đóng góp nhiều nhất một người hài lòng trong số tất cả$i$như vậy$a_i = v$. Vì vậy, điều tốt nhất chúng ta có thể làm là đếm xem có bao nhiêu ghế riêng biệt xuất hiện trong danh sách ưu tiên. 

Điều còn lại là liệu giới hạn trên này có luôn đạt được hay không. Đúng vậy. Chúng ta có thể chỉ định, cho từng chỗ ngồi riêng biệt$v$, chính xác là một người muốn, rồi quyết tâm ép buộc di chuyển ra ngoài. Những bước di chuyển bắt buộc này không bao giờ làm giảm số lượng vì chúng chỉ xác định vị trí của các nhiệm vụ không được chọn hoặc dự phòng chứ không phải số lượng nhiệm vụ ưu tiên mà chúng tôi đã ấn định. 

Do đó, câu trả lời giảm xuống số lượng giá trị riêng biệt trong$a$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tìm kiếm Brute Force qua bài tập | hàm mũ | O(n) | Quá chậm | 
| Đếm các sở thích riêng biệt | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta muốn tính xem có bao nhiêu chỗ ngồi khác nhau xuất hiện trong số tất cả các chỗ ngồi ưu tiên. 

1. Đọc tất cả các giá trị$a_1, a_2, \ldots, a_n$. Đây là những vị trí mục tiêu mà mọi người muốn chiếm giữ. 
2. Chèn mọi giá trị vào một tập hợp hoặc đánh dấu nó trong một mảng boolean. Điều này tự động loại bỏ các bản sao, đảm bảo mỗi chỗ ngồi chỉ được tính một lần. 
3. Kích thước của tập hợp này sau khi xử lý tất cả các giá trị là câu trả lời, vì mỗi chỗ ngồi riêng biệt có thể đóng góp tối đa một người hài lòng và giới hạn trên này luôn có thể được thực hiện. 

Việc triển khai không cần phải xây dựng chỗ ngồi cuối cùng hoặc mô phỏng các chuyển động vì cấu trúc của các xung đột được chuyển thành việc đếm tính duy nhất đơn giản. 

### Tại sao nó hoạt động 

Mỗi nhiệm vụ thành công cho một chỗ ngồi ưa thích sẽ tiêu tốn chỗ ngồi đó đúng một lần. Nếu hai hoặc nhiều người nhắm mục tiêu vào cùng một chỗ ngồi, thì chỉ một trong số họ có thể được chọn, do đó, các câu trả lời trùng lặp không thể làm tăng câu trả lời lên quá một chỗ cho mỗi chỗ ngồi. 

Đồng thời, việc chọn người đại diện cho từng ghế riêng biệt không bao giờ tạo ra mâu thuẫn cản trở làm giảm số lượng phân công ghế ưu tiên đã chọn. Bất kỳ điều chỉnh bắt buộc nào do giải phóng chỗ ngồi ban đầu đều chỉ ảnh hưởng đến vị trí dự phòng, không liên quan đến mục tiêu. 

Vì vậy, số lượng người hài lòng tối đa bằng số lượng ghế ưu tiên riêng biệt. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))

seen = set()
for x in a:
    seen.add(x)

print(len(seen))
```Giải pháp đọc tất cả các tùy chọn và lưu trữ chúng trong một bộ. Bộ tự động đảm bảo các bản sao không ảnh hưởng đến số lượng. Kích thước cuối cùng của bộ được in. 

Chi tiết triển khai tinh tế duy nhất là tránh mọi mô phỏng chuyển động của ghế không cần thiết. Mặc dù câu chuyện gợi ý sự phụ thuộc giữa mọi người khi có chỗ ngồi, nhưng những sự phụ thuộc đó không ảnh hưởng đến số lượng ghế ưu tiên có thể được phân bổ. 

## Ví dụ đã hoạt động 

Hãy xem xét một đầu vào trong đó một số người chia sẻ sở thích: 

đầu vào:$n = 5$,$a = [2, 2, 3, 3, 3]$| Bước | Giá trị đã xử lý | Đã xem bộ | 
| --- | --- | --- | 
| 1 | 2 | {2} | 
| 2 | 2 | {2} | 
| 3 | 3 | {2, 3} | 
| 4 | 3 | {2, 3} | 
| 5 | 3 | {2, 3} | 

Câu trả lời là 2, vì chỉ có ghế 2 và 3 được nhắm mục tiêu. Điều này phù hợp với ý tưởng rằng mỗi ghế có thể đóng góp nhiều nhất một người hài lòng. 

Bây giờ hãy xem xét một trường hợp không có sự lặp lại: 

đầu vào:$n = 4$,$a = [5, 6, 7, 8]$| Bước | Giá trị đã xử lý | Đã xem bộ | 
| --- | --- | --- | 
| 1 | 5 | {5} | 
| 2 | 6 | {5, 6} | 
| 3 | 7 | {5, 6, 7} | 
| 4 | 8 | {5, 6, 7, 8} | 

Tất cả các sở thích đều khác biệt, vì vậy mọi người đều có thể hài lòng. 

Những dấu vết này cho thấy rằng chỉ những vụ va chạm mới làm giảm số lượng có thể đạt được và mỗi vụ va chạm sẽ thu gọn thành một vị trí có thể sử dụng duy nhất trên mỗi ghế. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi tùy chọn được chèn vào bộ băm một lần | 
| Không gian | O(n) | Bộ lưu trữ lên đến$n$giá trị chỗ ngồi riêng biệt | 

Thuật toán dễ dàng phù hợp với các ràng buộc cho$n \le 10^5$, vì cả bộ nhớ và thời gian chạy đều có quy mô tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))
    return str(len(set(a)))

# provided sample-style checks
assert run("2\n1 2\n") == "2"
assert run("3\n1 1 1\n") == "1"

# custom cases
assert run("1\n5\n") == "1", "single element"
assert run("5\n1 2 3 4 5\n") == "5", "all distinct"
assert run("5\n2 2 2 2 2\n") == "1", "all equal"
assert run("6\n1 2 2 3 3 3\n") == "3", "mixed duplicates"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 1 | trường hợp ranh giới tối thiểu | 
| tất cả đều khác biệt | n | trường hợp sử dụng đầy đủ | 
| tất cả đều bình đẳng | 1 | trường hợp va chạm cực đại | 
| trùng lặp hỗn hợp | 3 | tính đúng đắn chung của phép trừ trùng lặp | 

## Vỏ cạnh 

Ví dụ: khi tất cả các tùy chọn đều giống hệt nhau$a = [7, 7, 7, 7]$, chỉ có một người có thể hài lòng. Thuật toán tạo ra một tập hợp có một phần tử, nắm bắt chính xác giới hạn này. 

Ví dụ: khi tất cả các tùy chọn đều khác biệt$a = [1, 2, 3, 4]$, mỗi người có thể được chỉ định chỗ ngồi mong muốn của mình. Bộ chứa tất cả các giá trị và câu trả lời bằng$n$. 

Khi tùy chọn kết hợp các giá trị trùng lặp và duy nhất, chẳng hạn như$a = [1, 2, 2, 3, 3]$, mỗi ghế trùng lặp chỉ đóng góp một lần. Việc xây dựng tập hợp sẽ thu gọn các bản sao một cách tự nhiên, đảm bảo không tính toán quá mức và không cần logic giải quyết xung đột rõ ràng.
