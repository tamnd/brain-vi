---
title: "CF 45E - Giám đốc"
description: "Chúng tôi có hai danh sách độc lập, một danh sách chứa tên và một danh sách chứa họ. Mỗi tên phải được ghép với đúng một họ và mỗi họ phải được sử dụng đúng một lần. Sau khi chọn kết quả phù hợp, chúng tôi in tất cả các cặp trong một dòng được phân tách bằng dấu phẩy."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy"]
categories: ["algorithms"]
codeforces_contest: 45
codeforces_index: "E"
codeforces_contest_name: "School Team Contest 3 (Winter Computer School 2010/11)"
rating: 2000
weight: 45
solve_time_s: 109
verified: true
draft: false
---

[CF 45E - Giám đốc](https://codeforces.com/problemset/problem/45/E) 

**Xếp hạng:** 2000 
**Tags:** thuật toán xây dựng, tham lam 
**Thời gian giải:** 1 phút 49s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có hai danh sách độc lập, một danh sách chứa tên và một danh sách chứa họ. Mỗi tên phải được ghép với đúng một họ và mỗi họ phải được sử dụng đúng một lần. Sau khi chọn kết quả phù hợp, chúng tôi in tất cả các cặp trong một dòng được phân tách bằng dấu phẩy. 

Mục tiêu tối ưu hóa có hai lớp. 

Đầu tiên, chúng tôi muốn càng nhiều cặp càng tốt trong đó tên và họ bắt đầu bằng cùng một chữ cái. 

Thứ hai, trong số tất cả các kết quả khớp với số lượng cặp như vậy tối đa có thể, chúng tôi muốn dòng in cuối cùng phải nhỏ nhất về mặt từ điển. 

Việc so sánh từ điển được thực hiện trên toàn bộ chuỗi cuối cùng, không phải trên từng cặp riêng lẻ. Vì các cặp được in theo thứ tự và được phân tách bằng các dấu phân cách cố định nên các cặp trước đó sẽ chiếm ưu thế trong so sánh. 

Hạn chế rất nhỏ, chỉ có tối đa 100 tên và 100 họ. Một tìm kiếm mạnh mẽ trên tất cả các hoán vị sẽ yêu cầu kiểm tra tới$100!$nhiệm vụ, điều đó hoàn toàn không thể thực hiện được. Ngay cả đối với$n = 12$, sự tăng trưởng giai thừa đã trở nên quá lớn. 

Giá trị nhỏ của$n$gợi ý rằng các thuật toán đa thức có độ phức tạp bậc ba hoặc bậc bốn là hoàn toàn ổn. Khó khăn thực sự không phải là hiệu suất mà là việc kết hợp hai mục tiêu tối ưu hóa một cách chính xác. 

Trường hợp khó nhận thấy đầu tiên là việc tối đa hóa các chữ cái đầu bằng nhau là không đủ. Các kết hợp tối ưu khác nhau có thể tạo ra các kết quả từ điển khác nhau. 

Ví dụ:```
2
Bob
Carl
Cooper
Brown
```Đầu ra tối ưu có thể có:```
Bob Brown, Carl Cooper
```Và```
Bob Cooper, Carl Brown
```Cái đầu tiên có hai chữ cái đầu giống nhau và nhỏ hơn về mặt từ điển. Thuật toán tham lam chỉ cố gắng tối đa hóa số lượng có thể vô tình chọn cách sắp xếp thứ hai. 

Một cái bẫy khác là cho rằng chúng ta có thể sắp xếp tên và họ một cách độc lập và ghép đôi một cách tham lam. 

Ví dụ:```
3
Anna
Bella
Clara
Carter
Adams
Brown
```Một cặp được sắp xếp ngây thơ sẽ mang lại:```
Anna Adams, Bella Brown, Clara Carter
```đó là tối ưu. 

Nhưng nếu chúng ta thay đổi họ một chút:```
3
Anna
Bella
Clara
Carter
Brown
Anderson
```Ghép nối được sắp xếp tham lam trở thành:```
Anna Anderson, Bella Brown, Clara Carter
```Vẫn tối ưu. 

Bây giờ hãy xem xét:```
3
Anna
Bella
Clara
Cooper
Brown
Adams
```Nếu chúng ta sớm tham lam sử dụng họ đẹp nhất, chúng ta có thể chặn các kết quả trùng khớp với cùng một chữ cái trong tương lai. Bài toán gán phụ thuộc toàn cục vào tất cả các lựa chọn còn lại. 

Phần khó nhất là sự ràng buộc về mặt từ điển. Ngay cả sau khi tìm thấy số lượng tên viết tắt trùng khớp tối đa, việc triển khai bất cẩn có thể tạo ra câu trả lời không tối thiểu vì nó tối ưu hóa các cặp cục bộ thay vì toàn bộ chuỗi được nối. 

## Phương pháp tiếp cận 

Giải pháp vũ phu thử mọi hoán vị của họ. Với mỗi hoán vị, chúng ta đếm xem có bao nhiêu cặp có cùng chữ cái đầu tiên. Trong số những người có điểm tối đa, chúng tôi giữ chuỗi kết quả nhỏ nhất về mặt từ điển. 

Cách tiếp cận này đúng vì nó kiểm tra rõ ràng mọi phép gán có thể. Vấn đề là thời gian chạy. có$n!$hoán vị. Ngay cả đối với$n = 15$, cái này đã có sẵn rồi$10^{12}$, hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là tiêu chí tối ưu hóa đầu tiên chỉ phụ thuộc vào tên viết tắt. Một cặp đóng góp 1 hoặc 0 tùy thuộc vào việc các chữ cái đầu tiên có khớp hay không. 

Điều này ngay lập tức biến vấn đề thành vấn đề đối sánh hai bên. 

Tạo một biểu đồ trong đó mọi tên có thể kết nối với những họ có cùng chữ cái bắt đầu. Khi đó, số lượng cặp tốt nhất chỉ đơn giản là kích thước của kết quả phù hợp tối đa trong biểu đồ này. 

Sau khi biết điểm tối ưu, chúng tôi vẫn cần kết quả cuối cùng nhỏ nhất về mặt từ điển. Phần này mang tính xây dựng và tham lam. 

Giả sử chúng ta xây dựng câu trả lời từ trái sang phải. Ở mỗi bước, chúng tôi thử các họ ứng cử viên cho tên hiện tại theo thứ tự từ điển của chuỗi cặp kết quả. Chúng tôi tạm thời sửa một cặp và hỏi: 

"Có thể đạt được tổng số chữ cái đầu phù hợp tối ưu với các phần tử không được sử dụng còn lại không?" 

Nếu có, chúng tôi cam kết với sự lựa chọn đó. Nếu không chúng tôi bỏ qua nó. 

Điều này có hiệu quả vì thứ tự từ điển được xác định bởi ký tự khác nhau sớm nhất. Sau khi chúng tôi sửa tiền tố khả thi nhỏ nhất thì không có quyết định nào sau này có thể cải thiện câu trả lời. 

Việc kiểm tra tính khả thi một lần nữa chỉ là sự đối sánh lưỡng cực tối đa trên biểu đồ còn lại. 

Từ$n \le 100$, chạy liên tục thuật toán Kuhn là đủ nhanh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n! \cdot n)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n^4)$|$O(n^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả tên và họ. 
2. Xây dựng biểu đồ hai bên trong đó tên`i`kết nối với họ`j`nếu chữ cái đầu tiên của chúng bằng nhau. 
3. Chạy đối sánh lưỡng cực tối đa trên biểu đồ này. 

Kích thước khớp kết quả là số lượng cặp tối đa có thể có tên viết tắt bằng nhau. 
4. Sắp xếp tên theo từ điển. 

Chuỗi in cuối cùng so sánh các cặp từ trái sang phải, vì vậy các tên trước đó chiếm ưu thế trong thứ tự từ điển. 
5. Xử lý từng tên một theo thứ tự sắp xếp. 
6. Đối với tên hiện tại, hãy thử tất cả các họ hiện không được sử dụng theo thứ tự từ điển của chuỗi cặp`"Name Surname"`. 
7. Tạm gán một họ cho tên hiện tại. 
8. Tính xem có bao nhiêu cặp ban đầu phù hợp đã được đảm bảo bởi các lựa chọn cố định. 
9. Trên những tên và họ còn lại chưa sử dụng, hãy chạy lại kết quả khớp tối đa. 
10. Nếu nhiệm vụ cố định hiện tại cộng với kết quả phù hợp nhất còn lại vẫn có thể đạt mức tối ưu toàn cầu, hãy giữ họ này vĩnh viễn. 

Nếu không thì hãy loại bỏ nó và thử họ tiếp theo. 
11. Tiếp tục cho đến khi mọi tên đã được chỉ định. 
12. In các cặp kết quả theo thứ tự đã chọn. 

### Tại sao nó hoạt động 

Thuật toán duy trì một bất biến quan trọng: 

Sau khi sửa lỗi đầu tiên$k$các cặp, vẫn tồn tại sự hoàn thành đạt được số lượng tên viết tắt phù hợp tối đa có thể. 

Khi chọn cặp tiếp theo, chúng tôi kiểm tra họ ứng viên theo thứ tự từ điển và chọn họ đầu tiên bảo toàn tính bất biến này. 

Giả sử tồn tại một giải pháp tối ưu nhỏ hơn về mặt từ điển. Sau đó, ở vị trí đầu tiên mà giải pháp của chúng tôi khác, giải pháp nhỏ hơn đó sẽ sử dụng cặp ứng cử viên mà chúng tôi đã từ chối trước đó. Nhưng chúng tôi chỉ từ chối một cặp nếu nó không thể đạt được điểm tối ưu. Điều này mâu thuẫn với giả định rằng giải pháp nhỏ hơn là tối ưu. 

Vì vậy, mọi tiền tố được chọn là tiền tố khả thi nhỏ nhất, đảm bảo toàn bộ chuỗi cuối cùng ở mức tối thiểu về mặt từ điển trong số tất cả các giải pháp tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def max_matching(names, surnames):
    n = len(names)
    m = len(surnames)

    g = [[] for _ in range(n)]

    for i in range(n):
        for j in range(m):
            if names[i][0] == surnames[j][0]:
                g[i].append(j)

    match_to = [-1] * m

    def dfs(v, vis):
        if vis[v]:
            return False

        vis[v] = True

        for to in g[v]:
            if match_to[to] == -1 or dfs(match_to[to], vis):
                match_to[to] = v
                return True

        return False

    res = 0

    for i in range(n):
        vis = [False] * n
        if dfs(i, vis):
            res += 1

    return res

def solve():
    n = int(input())

    names = [input().strip() for _ in range(n)]
    surnames = [input().strip() for _ in range(n)]

    names.sort()

    best = max_matching(names, surnames)

    used = [False] * n
    answer = []

    fixed_good = 0

    for idx, name in enumerate(names):
        candidates = []

        for j in range(n):
            if not used[j]:
                candidates.append((name + " " + surnames[j], j))

        candidates.sort()

        for pair_str, j in candidates:
            add = 1 if name[0] == surnames[j][0] else 0

            rem_names = []
            rem_surnames = []

            for k in range(idx + 1, n):
                rem_names.append(names[k])

            for k in range(n):
                if not used[k] and k != j:
                    rem_surnames.append(surnames[k])

            possible = fixed_good + add + max_matching(rem_names, rem_surnames)

            if possible == best:
                used[j] = True
                fixed_good += add
                answer.append(pair_str)
                break

    print(", ".join(answer))

solve()
```các`max_matching`hàm tính số lượng tối đa các cặp ban đầu bằng nhau trong số hai bộ còn lại. Vì đồ thị rất nhỏ nên việc so khớp Kuhn tiêu chuẩn là đủ. 

Biểu đồ được xây dựng lại từ đầu mỗi lần. Điều này có thể trông đắt tiền, nhưng giới hạn đủ nhỏ để sự đơn giản được ưu tiên hơn. 

Tên được sắp xếp trước khi bắt đầu xây dựng. Chi tiết này rất cần thiết vì bản thân thứ tự in ảnh hưởng đến việc so sánh từ điển. Nếu chúng ta xử lý tên theo thứ tự tùy ý thì chuỗi cuối cùng không nhất thiết phải là số tối thiểu. 

Đối với mỗi tên hiện tại, chúng tôi tạo ra tất cả các họ chưa được sử dụng và sắp xếp trực tiếp các chuỗi cặp ứng cử viên. Điều này tránh được những sai sót do cố gắng so sánh chỉ họ hoặc chỉ tên viết tắt. 

Kiểm tra tính khả thi là trung tâm của giải pháp. Sau khi tạm thời sửa một cặp, chúng tôi tính toán số lượng tên viết tắt trùng khớp bổ sung tối đa có thể đạt được trên các phần tử còn lại. Nếu chúng ta vẫn có thể đạt được mức tối ưu toàn cục thì sự lựa chọn là an toàn. 

Một chi tiết triển khai tinh tế đang được cập nhật`fixed_good`chỉ sau khi cam kết với một cặp. Cập nhật quá sớm sẽ làm hỏng việc kiểm tra tính khả thi trong tương lai. 

Một điểm tinh tế khác là chức năng so khớp hoạt động chính xác ngay cả khi một bên trống. Điều này tự nhiên xử lý lần lặp cuối cùng mà không có trường hợp đặc biệt. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
Ann
Anna
Sabrina
John
Petrov
Ivanova
Stoltz
Abacaba
```Tên được sắp xếp:```
Ann
Anna
John
Sabrina
```Kích thước phù hợp tối đa là 2. 

| Bước | Tên hiện tại | Họ được chọn | Phù hợp với tên ban đầu? | Đã sửa Tốt | 
| --- | --- | --- | --- | --- | 
| 1 | Ann | Abacaba | Có | 1 | 
| 2 | Anna | Ivanova | Không | 1 | 
| 3 | John | Petrov | Không | 1 | 
| 4 | Sabrina | Stoltz | Có | 2 | 

Đầu ra cuối cùng:```
Ann Abacaba, Anna Ivanova, John Petrov, Sabrina Stoltz
```Dấu vết này cho thấy việc xây dựng từ điển tham lam. Mặc dù`Anna Abacaba`hấp dẫn về mặt từ điển, sử dụng`Abacaba`ở đó sẽ ngăn chặn`Ann`để có được họ trùng khớp với tên viết tắt trong khi vẫn giữ câu trả lời ở mức tối thiểu. 

### Ví dụ 2 

đầu vào:```
3
Bob
Carl
Chris
Cooper
Brown
Adams
```Kích thước phù hợp tối đa là 2. 

| Bước | Tên hiện tại | Ứng viên đã thử | Khả thi? | Được chọn | 
| --- | --- | --- | --- | --- | 
| 1 | Bob | Bob Adams | Không | Không | 
| 1 | Bob | Bob Brown | Có | Có | 
| 2 | Carl | Carl Adams | Không | Không | 
| 2 | Carl | Carl Cooper | Có | Có | 
| 3 | Chris | Chris Adams | Có | Có | 

Đầu ra cuối cùng:```
Bob Brown, Carl Cooper, Chris Adams
```Ví dụ này chứng minh tại sao việc kiểm tra tính khả thi lại quan trọng. Lựa chọn`Bob Adams`ngay lập tức phá hủy khả năng đạt được điểm tối ưu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^4)$| Chúng tôi chạy$O(n^2)$kết hợp, mỗi lần lấy$O(n^2)$| 
| Không gian |$O(n^2)$| Lưu trữ đồ thị lưỡng cực | 

Với$n \le 100$, đại khái$10^8$các thao tác nhẹ có thể được chấp nhận trong Python, đặc biệt vì biểu đồ thưa thớt và đầu vào thực tế nhỏ hơn. Việc sử dụng bộ nhớ rất nhỏ so với giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    def max_matching(names, surnames):
        n = len(names)
        m = len(surnames)

        g = [[] for _ in range(n)]

        for i in range(n):
            for j in range(m):
                if names[i][0] == surnames[j][0]:
                    g[i].append(j)

        match_to = [-1] * m

        def dfs(v, vis):
            if vis[v]:
                return False

            vis[v] = True

            for to in g[v]:
                if match_to[to] == -1 or dfs(match_to[to], vis):
                    match_to[to] = v
                    return True

            return False

        res = 0

        for i in range(n):
            vis = [False] * n
            if dfs(i, vis):
                res += 1

        return res

    n = int(input())

    names = [input().strip() for _ in range(n)]
    surnames = [input().strip() for _ in range(n)]

    names.sort()

    best = max_matching(names, surnames)

    used = [False] * n
    answer = []

    fixed_good = 0

    for idx, name in enumerate(names):
        candidates = []

        for j in range(n):
            if not used[j]:
                candidates.append((name + " " + surnames[j], j))

        candidates.sort()

        for pair_str, j in candidates:
            add = 1 if name[0] == surnames[j][0] else 0

            rem_names = names[idx + 1:]

            rem_surnames = []

            for k in range(n):
                if not used[k] and k != j:
                    rem_surnames.append(surnames[k])

            possible = fixed_good + add + max_matching(rem_names, rem_surnames)

            if possible == best:
                used[j] = True
                fixed_good += add
                answer.append(pair_str)
                break

    return ", ".join(answer)

# provided sample
assert run(
"""4
Ann
Anna
Sabrina
John
Petrov
Ivanova
Stoltz
Abacaba
"""
) == "Ann Abacaba, Anna Ivanova, John Petrov, Sabrina Stoltz", "sample 1"

# minimum size
assert run(
"""1
Alice
Brown
"""
) == "Alice Brown", "n = 1"

# all initials match
assert run(
"""3
Amy
Bob
Carl
Adams
Brown
Cooper
"""
) == "Amy Adams, Bob Brown, Carl Cooper", "perfect matching"

# no initials match
assert run(
"""2
Alice
Bob
Cooper
Davis
"""
) == "Alice Cooper, Bob Davis", "lexicographic ordering only"

# greedy trap
assert run(
"""3
Bob
Carl
Chris
Cooper
Brown
Adams
"""
) == "Bob Brown, Carl Cooper, Chris Adams", "feasibility check"

print("All tests passed.")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Tên riêng và họ | Cặp chính xác | Ranh giới tối thiểu | 
| Mỗi trận đấu đầu tiên | Kết hợp hoàn hảo | Xử lý điểm tối đa | 
| Không có tên viết tắt phù hợp | Hành vi từ điển thuần túy | Tối ưu hóa thứ cấp | 
| Vụ án bẫy tham lam | Nhiệm vụ cụ thể | Kiểm tra tính khả thi chính xác | 

## Vỏ cạnh 

Hãy xem xét tình huống trong đó một lựa chọn cục bộ nhỏ hơn về mặt từ điển sẽ làm hỏng điểm tối ưu. 

đầu vào:```
3
Bob
Carl
Chris
Cooper
Brown
Adams
```Thuật toán đầu tiên kiểm tra`Bob Adams`. Điều này tạo ra không có chữ cái đầu phù hợp. Những cái tên còn lại là`Carl`Và`Chris`, trong khi các họ còn lại là`Cooper`Và`Brown`. Vẫn chỉ có thể có một cặp ban đầu phù hợp. 

Vì vậy, tổng số điểm có thể đạt được sẽ là 1, trong khi mức tối ưu toàn cầu là 2. Thuật toán sẽ bác bỏ lựa chọn này. 

Tiếp theo nó cố gắng`Bob Brown`, bảo toàn khả năng đạt được tổng cộng 2 trận đấu. Vì vậy, nó cam kết với cặp đó. 

Bây giờ hãy xem xét trường hợp không có tên viết tắt phù hợp nào có thể xảy ra. 

đầu vào:```
2
Alice
Bob
Cooper
Davis
```Kích thước phù hợp tối đa là 0. 

cho`Alice`, cả hai họ ứng cử viên đều giữ được số điểm tối ưu vì dù sao cũng không có cặp tốt nào tồn tại. Thuật toán chọn cặp nhỏ hơn về mặt từ điển,`Alice Cooper`. 

Sau đó chỉ`Bob Davis`vẫn còn. 

Đầu ra trở thành:```
Alice Cooper, Bob Davis
```Điều này xác nhận rằng khi mục tiêu chính được gắn ở mọi nơi, thuật toán sẽ giảm thiểu tối đa từ điển xuống mức tối thiểu. 

Cuối cùng, hãy xem xét nhiều khả năng ban đầu bằng nhau. 

đầu vào:```
3
Anna
Amy
Alex
Arnold
Adams
Avery
```Mỗi bài tập đều đạt được số điểm tối đa 3. 

Thuật toán xử lý tên theo thứ tự sắp xếp:```
Alex
Amy
Anna
```Vì`Alex`, cặp khả thi nhỏ nhất là`Alex Adams`. 

Sau đó cho`Amy`, cặp khả thi nhỏ nhất còn lại là`Amy Arnold`. 

Cuối cùng`Anna Avery`. 

Kết quả:```
Alex Adams, Amy Arnold, Anna Avery
```Bởi vì mọi lựa chọn một phần vẫn khả thi nên thuật toán luôn chọn tiền tố nhỏ nhất có sẵn, khớp chính xác với thứ tự từ điển.
