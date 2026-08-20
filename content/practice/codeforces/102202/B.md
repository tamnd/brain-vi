---
title: "CF 102202B - Gosu"
description: "Kết quả trận đấu tạo thành một biểu đồ giải đấu trực tiếp. Mỗi học sinh là một đỉnh và với mỗi cặp học sinh khác nhau tồn tại một cạnh có hướng: nếu học sinh (i) đánh bại học sinh (j) thì sẽ có một cạnh (i đến j). Đường dẫn chiến thắng chỉ đơn giản là đường dẫn được định hướng trong biểu đồ này."
date: "2026-08-18T01:03:11+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102202
codeforces_index: "B"
codeforces_contest_name: "2019 KAIST RUN Spring Contest"
rating: 0
weight: 102202
solve_time_s: 201
verified: false
draft: false
---

[CF 102202B - Gosu](https://codeforces.com/problemset/problem/102202/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 21s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Kết quả trận đấu tạo thành một biểu đồ giải đấu trực tiếp. Mỗi học sinh là một đỉnh và với mỗi cặp học sinh khác nhau tồn tại một cạnh có hướng: nếu học sinh (i) đánh bại học sinh (j), thì sẽ có một cạnh (i \to j). 

Đường dẫn chiến thắng chỉ đơn giản là đường dẫn được định hướng trong biểu đồ này. Khoảng cách từ (x) đến (y) là độ dài đường dẫn có hướng ngắn nhất, có giá trị đặc biệt (9000) khi không thể truy cập được (y). Điểm yếu của một học sinh chính là khoảng cách lớn nhất giữa học sinh đó với bất kỳ học sinh nào. Chúng ta cần xuất ra một học sinh có điểm yếu càng nhỏ càng tốt, cùng với điểm yếu tối thiểu đó. 

Đầu vào là một ma trận ký tự (N \times N). Hàng (i) mô tả mọi trận đấu liên quan đến học sinh (i), trong đó`W`có nghĩa là cạnh đi từ (i) tới sinh viên của cột đó và`L`có nghĩa là cạnh đi theo hướng ngược lại. Vì mỗi cặp chơi đúng một lần nên đây là một giải đấu chứ không phải là một biểu đồ có hướng tùy ý. 

Ràng buộc (N \le 3000) là đầu mối trung tâm. Đọc toàn bộ ma trận đã tốn chi phí (O(N^2)), tức là khoảng chín triệu ký tự ở kích thước tối đa và hoàn toàn hợp lý. Tuy nhiên, thuật toán (O(N^3)) thực hiện tới (27) tỷ thao tác đồ thị cơ bản khi (N=3000), vượt xa giới hạn thời gian một giây có thể chịu đựng được. Chúng ta cần trích xuất thêm thông tin trực tiếp từ cấu trúc giải đấu thay vì tính toán đường đi ngắn nhất cho tất cả các cặp. 

Có hai trường hợp dễ xảy ra là một giải pháp bất cẩn có thể xử lý sai. Đầu tiên, khoảng cách từ một học sinh đến chính mình là bằng 0 chứ không phải một. Ví dụ,```
2
-W
L-
```Học sinh 1 đánh trực tiếp học sinh 2 nên khoảng cách với học sinh 1 là (0,1) cho điểm yếu (1). Đầu ra đúng là`1 1`. Giải pháp tính đỉnh bắt đầu là yêu cầu một bước có thể báo cáo không chính xác (2). 

Trường hợp thứ hai là không ai trực tiếp đánh bại tất cả mọi người. Hãy xem xét giải đấu theo chu kỳ```
3
-LW
W-L
LW-
```Học sinh 1 thua học sinh 2, nhưng học sinh 1 đánh bại học sinh 3 và học sinh 3 đánh bại học sinh 2. Như vậy, học sinh 1 sẽ đánh bại mọi học sinh trong vòng hai trận thắng, dẫn đến điểm yếu (2). Đầu ra đúng là`2 1`. Chỉ cần chọn học sinh có số lần thắng trực tiếp lớn nhất và giả sử câu trả lời luôn là (1) sẽ thất bại ở đây. 

Điều tinh tế thứ ba là giá trị đặc biệt (9000) không bao giờ cần xuất hiện trong câu trả lời. Một giải đấu luôn có một học sinh có thể tiếp cận mọi học sinh khác ở nhiều nhất hai cạnh có hướng. Giải pháp dưới đây xây dựng trực tiếp một học sinh như vậy, vì vậy điểm yếu tối ưu luôn là (1) hoặc (2). 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là xử lý kết quả dưới dạng biểu đồ có hướng và thực hiện tìm kiếm theo chiều rộng từ mọi học sinh. Một BFS tính toán tất cả các khoảng cách ngắn nhất từ ​​​​một học sinh bắt đầu, vì vậy sau (N) BFS chạy, chúng tôi biết điểm yếu của mọi học sinh và có thể chọn điểm yếu nhỏ nhất. Với ma trận kề, mỗi BFS có thể kiểm tra (N) các cạnh đi ra có thể có cho mỗi (N) đỉnh, đưa ra (O(N^2)) công việc trên mỗi nguồn và (O(N^3)) tổng thể. Tại (N=3000), con số này lên tới (3000^3 = 27{,}000{,}000{,}000) lần kiểm tra kề cận, quá chậm. 

Phương pháp brute-force hoạt động vì BFS khớp chính xác với định nghĩa về khoảng cách đường dẫn chiến thắng. Vấn đề là giải đấu có cấu trúc mạnh hơn nhiều so với đồ thị có hướng chung. 

Quan sát quan trọng là một đỉnh có bậc ngoài tối đa luôn có thể đến mọi đỉnh khác trong tối đa hai bước. Ở đây, mức độ ngoài chỉ đơn giản là số lượng`W`các ký tự trong hàng của học sinh đó. 

Lấy một sinh viên có trình độ tối đa (v) và xem xét một sinh viên khác (u) mà (v) thua. Nếu (v) không thể đạt được (u) trong hai bước thì sẽ không có học sinh (w) sao cho (v) đánh bại (w) và (w) đánh bại (u). Vì mỗi cặp có chính xác một người chiến thắng nên mọi học sinh bị (v) đánh bại thì phải bị (u) đánh bại. 

Điều đó mang lại sự mâu thuẫn. Học sinh (u) đánh bại (v) và (u) cũng đánh bại mọi học sinh mà (v) đánh bại. Do đó, số lần thắng trực tiếp của (u) lớn hơn rất nhiều so với (v), mâu thuẫn với việc chọn (v) là một sinh viên có trình độ tối đa. 

Vì vậy, mọi học sinh đều thua trực tiếp với (v), trong trường hợp đó có một con đường hai cạnh thông qua một số học sinh trung cấp hoặc đã bị (v đánh bại trực tiếp). Do đó điểm yếu của một sinh viên có trình độ tối đa là nhiều nhất (2). 

Điều này làm giảm toàn bộ vấn đề về việc tính số tiền thắng trực tiếp. Nếu một số học sinh có (N-1) thắng thì điểm yếu của họ chính xác là (1), bởi vì mọi học sinh khác đều được tiếp cận trực tiếp. Ngược lại, không có điểm yếu nào của học sinh có thể là (1), trong khi học sinh có trình độ tối đa có điểm yếu nhiều nhất là (2). Do đó, điểm yếu của nó chính xác là (2). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force với BFS từ mọi học sinh | (O(N^3)) | (O(N^2)) | Quá chậm | 
| Mức độ tối đa | (O(N^2)) | (O(N^2)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc ma trận kết quả đầy đủ. Chúng ta cần kiểm tra từng ký tự vì số lần thắng trực tiếp của mỗi học sinh sẽ quyết định ứng cử viên. 
2. Đếm xem mỗi hàng có bao nhiêu ký tự`W`. Số này là cấp độ ngoại hạng của học sinh, nghĩa là số đối thủ mà học sinh đó trực tiếp đánh bại. 
3. Giữ học sinh có bằng cấp lớn nhất. Chúng ta chưa cần xây dựng bất kỳ đường dẫn nào vì thuộc tính mức độ tối đa đã đảm bảo rằng sinh viên này sẽ tiếp cận mọi người trong vòng hai bước. 
4. Nếu mức độ vượt trội tối đa là (N-1), điểm yếu đầu ra (1) và học sinh đó. Mọi học sinh khác đều là hàng xóm trực tiếp đi ra ngoài, do đó không cần khoảng cách dương lớn hơn một. 
5. Ngược lại, điểm yếu đầu ra (2) và cùng một học sinh. Lập luận mức độ tối đa chứng minh rằng mọi đối thủ không bị đánh trực tiếp đều có thể tiếp cận được thông qua một học sinh trung cấp. Vì học sinh được chọn không trực tiếp đánh bại tất cả mọi người nên điểm yếu của nó không thể là (1), nên chính xác là (2). 

### Tại sao nó hoạt động 

Giả sử (v) là một sinh viên có bằng cấp tối đa. Giả sử (v) thua một học sinh nào đó (u). Giả sử mâu thuẫn rằng (v) không thể tới (u) trong hai bước. Đối với mọi học sinh (w) mà (v) đánh bại, (u) cũng phải đánh bại (w), vì nếu không thì (v \to w \to u) sẽ là một đường dẫn hai bước hợp lệ. Vì (u) còn đánh bại (v) nên mọi chiến thắng trực tiếp của (v) cũng là chiến thắng trực tiếp của (u), cộng với chiến thắng của (u) trước (v). Do đó (u) có bậc ngoài lớn hơn (v), mâu thuẫn với cực đại. 

Vì vậy mọi đỉnh đều cách (v một khoảng cách tối đa là hai). Nếu (v) có (N-1) thắng trực tiếp thì điểm yếu của nó là (1). Nếu nó có ít hơn thì điểm yếu của nó ít nhất là (2) và đối số ở trên đưa ra giới hạn trên là (2). Do đó, điểm yếu của học sinh được chọn chính xác là giá trị tối thiểu có thể có. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())

    best_player = 0
    best_wins = -1

    for i in range(n):
        row = input().strip()
        wins = row.count('W')

        if wins > best_wins:
            best_wins = wins
            best_player = i

    weakness = 1 if best_wins == n - 1 else 2

    print(weakness, best_player + 1)

if __name__ == "__main__":
    solve()
```Vòng lặp đầu vào đọc từng hàng hoàn chỉnh và sử dụng`count('W')`để lấy được bằng cấp ngoài của sinh viên. Đường chéo`-`và tất cả`L`các ký tự không cần xử lý đặc biệt vì không thể hiện chiến thắng trực tiếp của học sinh ở hàng hiện tại. 

Việc so sánh sử dụng`>`còn hơn là`>=`. Lựa chọn nào cũng đúng vì được phép có nhiều sinh viên có trình độ tối đa, nhưng sử dụng`>`chỉ đơn giản là giữ lại học sinh đầu tiên như vậy. 

Bài kiểm tra cuối cùng là`best_wins == n - 1`, không`best_wins == n`. Một học sinh không thể tự đánh bại mình nên chỉ có (N-1) khả năng thắng trực tiếp. Nếu điều kiện đó đúng thì mọi đỉnh khác đều đạt đến một cạnh. Ngược lại, định lý mức độ cực đại cho điểm yếu đúng bằng hai. 

Câu trả lời chuyển đổi chỉ số Python dựa trên 0 trở lại cách đánh số học sinh dựa trên một mà bài toán yêu cầu. 

Không có vấn đề tràn số nguyên trong Python và số lượng lớn nhất được lưu trữ trong`best_wins`chỉ là (N-1). Bản thân ma trận không cần phải được lưu trữ sau khi xử lý mỗi hàng, điều này giúp cho việc sử dụng bộ nhớ bổ sung rất nhỏ. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Dữ liệu đầu vào mô tả chiến thắng trực tiếp từ học sinh 1 đến học sinh 2. 

| Sinh viên | Hàng | Thắng trực tiếp | Tốt nhất sau hàng | Quyết định điểm yếu | 
| --- | --- | --- | --- | --- | 
| 1 |`-W`| 1 | sinh viên 1, 1 thắng | đang chờ xử lý | 
| 2 |`L-`| 0 | sinh viên 1, 1 thắng | đang chờ xử lý | 
| Cuối cùng | | | sinh viên 1 | (1), vì (1=N-1) | 

Học sinh 1 có một trận thắng trực tiếp và (N-1=1) là số điểm tối đa có thể. Như vậy mọi đối thủ đều có thể tiếp cận trực tiếp nên điểm yếu là (1), đưa ra`1 1`. 

Ví dụ này thể hiện ranh giới giữa khoảng cách bản thân có độ dài bằng 0 và khoảng cách một cạnh. Bài dự thi của chính học sinh không đóng góp vào số chiến thắng. 

### Mẫu 2 

Ma trận kết quả là```
-LW
W-L
LW-
```Học sinh 1 thắng học sinh 3 nhưng thua học sinh 2. Học sinh 2 cũng có một trận thắng trực tiếp và học sinh 3 có một trận thắng trực tiếp. 

| Sinh viên | Hàng | Thắng trực tiếp | Tốt nhất sau hàng | Quyết định điểm yếu | 
| --- | --- | --- | --- | --- | 
| 1 |`-LW`| 1 | sinh viên 1, 1 thắng | đang chờ xử lý | 
| 2 |`W-L`| 1 | sinh viên 1, 1 thắng | đang chờ xử lý | 
| 3 |`LW-`| 1 | sinh viên 1, 1 thắng | đang chờ xử lý | 
| Cuối cùng | | | sinh viên 1 | (2), vì (1<N-1) | 

Đối với học sinh 1, đường đi đến học sinh 2 có độ dài 2: (1 \to 3 \to 2). Định lý mức độ cực đại đảm bảo một đường đi như vậy mà không yêu cầu chúng ta phải tìm kiếm nó một cách rõ ràng. 

Không có học sinh nào thắng trực tiếp hai lần nên điểm yếu (1) là không thể. Học sinh 1 tiếp cận mọi người trong vòng hai bước, vì vậy điểm yếu tối thiểu chính xác là (2). Đầu ra`2 1`là hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(N^2)) | Mỗi một trong số (N^2) ký tự ma trận được đọc hoặc kiểm tra một lần | 
| Không gian | (O(N)) | Một hàng đầu vào được lưu trữ tại một thời điểm, chỉ có một vài bộ đếm và chỉ số | 

Tại (N=3000), (N^2=9{,}000{,}000), điều này thực tế trong các giới hạn đã cho. Thuật toán tránh được (27) tỷ hoạt động mà cách tiếp cận BFS tất cả các nguồn có thể yêu cầu. Việc triển khai chuỗi của Python cũng làm cho việc đếm`W`các ký tự trong mỗi hàng đủ hiệu quả cho kích thước đầu vào này. 

## Trường hợp thử nghiệm 

Khai thác thử nghiệm sau đây triển khai thuật toán tương tự như một hàm để có thể kiểm tra kết quả trả về. Vì vấn đề cho phép bất kỳ Gosu nào khi có một số Gosu tồn tại, nên người xác nhận sẽ kiểm tra xem học sinh được báo cáo có điểm yếu được xác nhận hay không thay vì yêu cầu một chỉ số học sinh chính xác.```python
import sys
import io

def solve_data(inp: str) -> str:
    data = inp.strip().splitlines()
    n = int(data[0])

    best_player = 0
    best_wins = -1

    for i in range(n):
        wins = data[i + 1].count('W')
        if wins > best_wins:
            best_wins = wins
            best_player = i

    weakness = 1 if best_wins == n - 1 else 2
    return f"{weakness} {best_player + 1}"

def brute_weakness(rows, start):
    n = len(rows)
    dist = [-1] * n
    dist[start] = 0
    queue = [start]

    for v in queue:
        for u in range(n):
            if rows[v][u] == 'W' and dist[u] == -1:
                dist[u] = dist[v] + 1
                queue.append(u)

    if -1 in dist:
        return 9000
    return max(dist)

def check(inp: str, out: str):
    lines = inp.strip().splitlines()
    n = int(lines[0])
    rows = lines[1:]

    weakness, player = map(int, out.split())
    assert 1 <= player <= n
    assert weakness == brute_weakness(rows, player - 1)

    all_weaknesses = [brute_weakness(rows, i) for i in range(n)]
    assert weakness == min(all_weaknesses)

def run(inp: str) -> str:
    out = solve_data(inp)
    check(inp, out)
    return out

# Provided samples
run("""2
-W
L-
""")

run("""3
-LW
W-L
LW-
""")

run("""5
-WLLW
L-LLW
WW-LL
WWW-W
LLWL-
""")

# Minimum size
run("""2
-W
L-
""")

# Three-cycle, where every student has weakness 2
run("""3
-LW
W-L
LW-
""")

# Transitive tournament, where student 1 beats everyone directly
run("""4
-WWW
L-WW
LL-W
LLL-
""")

# Maximum-size input, a transitive tournament.
# Student 1 beats everybody, so the answer must have weakness 1.
n = 3000
rows = []
for i in range(n):
    row = ['-'] * n
    for j in range(i + 1, n):
        row[j] = 'W'
        row[i] = 'L'
    rows.append(''.join(row))

large_input = str(n) + '\n' + '\n'.join(rows) + '\n'
large_output = solve_data(large_input)
assert large_output == "1 1", "maximum-size transitive tournament"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 / -W / L-`|`1 1`| Tối thiểu (N), người chiến thắng trực tiếp và xử lý chính xác đường chéo | 
|`3 / -LW / W-L / LW-`|`2 1`hoặc Gosu hợp lệ khác | Một giải đấu không có người chiến thắng trực tiếp chung | 
|`4 / -WWW / L-WW / LL-W / LLL-`|`1 1`| Trường hợp ranh giới trong đó mức độ ngoài tối đa là (N-1) | 
| Giải đấu chuyển tiếp với (N=3000) |`1 1`| Kích thước đầu vào tối đa và khả năng mở rộng (O(N^2)) | 

Một giải đấu "tất cả các giá trị bằng nhau" theo đúng nghĩa đen là không thể xảy ra đối với (N>2), bởi vì mỗi cặp phải có chính xác một người thắng và một người thua. Bài kiểm tra phù hợp nhất là bài kiểm tra ba chu kỳ, trong đó mỗi học sinh có đúng một lần thắng trực tiếp và số lần thắng trực tiếp bằng nhau. Thuật toán vẫn phải chọn một học sinh hợp lệ và trả về điểm yếu chính xác (2). 

## Vỏ cạnh 

Đối với giải đấu quy mô tối thiểu```
2
-W
L-
```bằng cấp ngoài của sinh viên đầu tiên là (1=N-1), trong khi bằng cấp ngoài của sinh viên thứ hai bằng 0. Thuật toán chọn sinh viên 1 và trả về điểm yếu (1). Khoảng cách bản thân bằng 0 và khoảng cách duy nhất còn lại là 1, do đó kết quả khớp chính xác với định nghĩa. 

Đối với một giải đấu không có người chiến thắng chung cuộc,```
3
-LW
W-L
LW-
```cả ba sinh viên đều có bằng cấp một. Thuật toán giữ sinh viên là 1 vì đây là mức tối đa đầu tiên. Học sinh 1 trực tiếp đến học sinh 3 và đến học sinh 2 đến học sinh 3 nên điểm yếu của nó là (2). Vì không có học sinh nào có (N-1=2) thắng trực tiếp nên điểm yếu (1) là không thể. 

Đối với trường hợp ranh giới trong đó một học sinh thắng mọi trận đấu,```
4
-WWW
L-WW
LL-W
LLL-
```học sinh 1 có bằng cấp 3, bằng (N-1). Thuật toán chỉ định ngay điểm yếu (1). Bằng cấp cao hơn của mọi sinh viên khác đều nhỏ hơn, vì vậy việc chọn sinh viên 1 rõ ràng cũng là tối ưu. 

Trường hợp kích thước tối đa chứa 3000 sinh viên. Một giải đấu chuyển tiếp có thể được hình thành bằng cách bắt học sinh (i) đánh bại mọi học sinh có chỉ số lớn hơn. Sinh viên 1 khi đó có 2999 trận thắng trực tiếp nên thuật toán trả về`1 1`. Việc triển khai xử lý chính xác 3000 hàng và đếm chúng`W`ký tự, tổng cộng khoảng chín triệu mục ma trận. Nó không bao giờ thực hiện BFS hoặc xây dựng ma trận khoảng cách, đó là lý do tại sao phương pháp bậc hai vẫn thực tế ở giới hạn trên.
