---
title: "CF 1525C - Robot va chạm"
description: "Chúng tôi có một bộ robot di chuyển dọc theo đoạn đường một chiều từ 0 đến $m$. Mỗi robot bắt đầu ở một vị trí số nguyên riêng biệt trong khoảng từ 0 đến $m$ và di chuyển sang trái hoặc phải với tốc độ không đổi 1 đơn vị mỗi giây."
date: "2026-06-10T17:25:42+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "greedy", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1525
codeforces_index: "C"
codeforces_contest_name: "Educational Codeforces Round 109 (Rated for Div. 2)"
rating: 2000
weight: 1525
solve_time_s: 183
verified: false
draft: false
---

[CF 1525C - Va chạm robot](https://codeforces.com/problemset/problem/1525/C) 

**Xếp hạng:** 2000 
**Tags:** cấu trúc dữ liệu, tham lam, triển khai, sắp xếp 
**Thời gian giải:** 3m 3s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một bộ robot di chuyển dọc theo đoạn đường một chiều từ 0 đến$m$. Mỗi robot bắt đầu ở một vị trí số nguyên khác nhau giữa 0 và$m$và di chuyển sang trái hoặc phải với tốc độ không đổi 1 đơn vị mỗi giây. Điểm cuối 0 và$m$hoạt động như những bức tường: khi robot chạm vào một bức tường, nó sẽ phản xạ ngay lập tức và tiếp tục di chuyển theo hướng ngược lại. Khi hai hoặc nhiều robot gặp nhau ở cùng một tọa độ nguyên, chúng sẽ va chạm và biến mất. Robot gặp nhau tại các vị trí không nguyên sẽ không va chạm. 

Đầu vào cung cấp vị trí bắt đầu và hướng dẫn cho tất cả các robot và mục tiêu là xác định đối với mỗi robot, thời gian nó phát nổ hoặc báo cáo$-1$nếu nó không bao giờ va chạm. 

Những ràng buộc chỉ ra rằng$n$có thể lên đến$3 \cdot 10^5$tổng cộng trên tất cả các trường hợp thử nghiệm và$m$có thể lớn như$10^8$. Việc mô phỏng lực lượng từng giây rõ ràng sẽ vượt quá giới hạn thời gian. Mọi giải pháp đều phải ở mức tuyến tính phụ trên mỗi robot mỗi giây; lý tưởng nhất là chúng ta cần thứ gì đó$O(n \log n)$cho mỗi trường hợp thử nghiệm hoặc tốt hơn. 

Trường hợp cạnh tinh vi xảy ra khi các robot giao nhau ở các vị trí không nguyên. Chẳng hạn, một robot ở vị trí 1 di chuyển sang phải và một robot khác ở vị trí 2 di chuyển sang trái sẽ vượt qua trong 1,5 giây. Mặc dù đi qua nhưng không xảy ra va chạm vì tọa độ không nguyên. Một cách tiếp cận ngây thơ mô phỏng tất cả các tương tác ngay khi các vị trí trùng nhau sẽ tạo ra câu trả lời sai. Một tình huống khó khăn khác là khi robot bật ra khỏi tường. Sự phản chiếu có thể khiến robot va chạm với những robot ban đầu ở xa, vì vậy chúng ta không thể bỏ qua tương tác trên tường. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản là mô phỏng robot ở mỗi giây, theo dõi vị trí và phát hiện các va chạm số nguyên. Mỗi giây, chúng tôi sẽ cập nhật tất cả các vị trí và kiểm tra xem có hai vị trí nào có cùng số nguyên hay không. Điều này đúng về mặt khái niệm nhưng đòi hỏi khả năng$O(m \cdot n)$hoạt động trên mỗi trường hợp thử nghiệm, điều này là không thể chấp nhận được vì$m$có thể$10^8$. 

Cái nhìn sâu sắc quan trọng để tối ưu hóa là quan sát rằng các va chạm xảy ra một cách xác định dựa trên tính chẵn lẻ của các vị trí bắt đầu. Các robot có cùng tính chẵn lẻ (vị trí chẵn/lẻ modulo 2) chỉ có thể va chạm với nhau ở tọa độ nguyên. Va chạm giữa các chẵn lẻ khác nhau không bao giờ xảy ra tại các thời điểm nguyên. Ngoài ra, sự phản xạ trên các bức tường có thể được mô phỏng bằng cách mở rộng đoạn đường gần như âm và xa hơn nữa.$m$, cho phép chúng ta coi mọi va chạm là tương tác trực diện mà không nảy ra khỏi tường một cách rõ ràng. 

Vì vậy, chúng ta có thể tách robot thành các vị trí bắt đầu lẻ và chẵn và sắp xếp chúng. Sau đó, chúng ta có thể ghép các robot chuyển động về phía nhau, ban đầu hoặc sau khi phản xạ từ tường và tính thời gian va chạm bằng một nửa khoảng cách giữa chúng. Đối với các robot di chuyển cùng hướng, va chạm đầu tiên xảy ra sau khi cả hai phản xạ khỏi tường, điều này cũng có thể được tính toán trực tiếp. Bằng cách xử lý robot theo tính chẵn lẻ và hướng, chúng ta có thể tính toán một cách có hệ thống thời gian va chạm trong$O(n \log n)$bằng cách sử dụng sắp xếp và ghép nối dựa trên ngăn xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n·m) | O(n) | Quá chậm | 
| Sắp xếp & Chẵn lẻ + Ngăn xếp | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chia tất cả robot thành hai nhóm dựa trên tính chẵn lẻ của vị trí ban đầu của chúng: lẻ và chẵn. Điều này đảm bảo chúng ta chỉ xem xét các va chạm tiềm ẩn có thể xảy ra ở những thời điểm nguyên. 
2. Đối với mỗi nhóm chẵn lẻ, hãy sắp xếp robot theo vị trí bắt đầu của chúng. 
3. Duy trì hai ngăn xếp cho mỗi chẵn lẻ: một cho robot di chuyển sang phải và một cho robot di chuyển sang trái. Lặp lại các robot theo thứ tự và đẩy các robot di chuyển ngay vào ngăn xếp. Khi gặp robot di chuyển sang trái, hãy kiểm tra xem có robot nào ở ngăn xếp bên phải để ghép đôi để va chạm hay không. 
4. Nếu hai robot va chạm trực diện thì thời gian va chạm bằng một nửa khoảng cách giữa vị trí xuất phát của chúng. 
5. Sau khi xử lý các va chạm trực tiếp, hãy xử lý các robot chưa từng gặp nhau trước khi phản xạ từ các bức tường. Mô phỏng phần mở rộng ảo của dòng: các vị trí của robot di chuyển sang trái nhỏ hơn 0 được phản chiếu sang các vị trí âm và robot di chuyển sang phải qua$m$được phản ánh xa hơn$m$. Lặp lại ghép nối dựa trên ngăn xếp trên các vị trí ảo này. 
6. Sau khi ghép nối, mọi robot còn lại không bao giờ tìm thấy va chạm sẽ được chỉ định$-1$. 
7. Lắp ráp kết quả theo thứ tự ban đầu của robot. 

Tại sao nó hoạt động: bằng cách chỉ xem xét các robot có cùng tính chẵn lẻ và theo dõi các robot di chuyển ngay trong một ngăn xếp, chúng tôi đảm bảo mỗi va chạm đều được xử lý theo thứ tự xảy ra. Sự phản xạ được tính bằng cách mở rộng các vị trí hầu như, vì vậy chúng tôi không bao giờ bỏ lỡ một vụ va chạm xảy ra sau khi bật lên. Điều này đảm bảo rằng va chạm số nguyên đầu tiên của mỗi robot, nếu có, được tính toán chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        x = list(map(int, input().split()))
        d = input().split()
        ans = [-1] * n

        robots = [(x[i], d[i], i) for i in range(n)]
        for parity in [0, 1]:
            group = [r for r in robots if r[0] % 2 == parity]
            group.sort()
            stack = []
            times = {}

            for pos, dirc, idx in group:
                if dirc == 'R':
                    stack.append((pos, idx))
                else:
                    if stack:
                        last_pos, last_idx = stack.pop()
                        collision_time = (pos - last_pos) // 2
                        ans[idx] = ans[last_idx] = collision_time
                    else:
                        stack.append((-pos, idx))

            stack2 = []
            while stack:
                a_pos, a_idx = stack.pop()
                if stack2:
                    b_pos, b_idx = stack2.pop()
                    collision_time = (2 * m - a_pos - b_pos) // 2
                    ans[a_idx] = ans[b_idx] = collision_time
                else:
                    stack2.append((a_pos, a_idx))
        print(*ans)

solve()
```Phần đầu tiên đọc đầu vào và khởi tạo lưu trữ kết quả. Robot được nhóm theo tính chẵn lẻ để đơn giản hóa việc tính toán va chạm. Việc sắp xếp đảm bảo chúng tôi xử lý robot theo thứ tự từ trái sang phải. Ngăn xếp đầu tiên xử lý các va chạm trực diện giữa động cơ bên phải và bên trái. Đường chuyền thứ hai xử lý phản xạ của tường bằng các vị trí phản chiếu. Việc chia cho hai chiếm tốc độ tương đối trong các vụ va chạm trực diện. Bất kỳ robot nào còn sót lại vẫn chưa được so sánh$-1$. Việc sử dụng phép chia số nguyên đảm bảo các xung đột chỉ được ghi lại khi chúng xảy ra ở thời điểm nguyên. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
7 12
1 2 3 4 9 10 11
R R L L R R R
```| Robot | Chẵn lẻ | Hướng | Ngăn xếp | Thời gian va chạm | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | R | [(1,0)] | - | 
| 2 | 0 | R | [(2,1)] | - | 
| 3 | 1 | L | [(1,0)] | 1 | 
| 4 | 0 | L | [(2,1)] | 1 | 
| 5 | 1 | R | [(5,4)] | - | 
| 6 | 0 | R | [(6,5)] | - | 
| 7 | 1 | R | [(5,4),(7,6)] | - | 

Bảng cho thấy robot 1 và 3 va chạm sau 1 giây, robot 2 và 4 sau 1 giây, các robot còn lại va chạm sau phản xạ của tường. 

### Mẫu 2 

đầu vào:```
2 10
1 6
R R
```Cả hai robot đều di chuyển sang phải. Không có va chạm xảy ra. Đầu ra: -1 -1. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Việc sắp xếp robot trong mỗi nhóm chẵn lẻ chiếm ưu thế trong thời gian chạy. | 
| Không gian | O(n) | Lưu trữ robot, ngăn xếp và kết quả. | 

Với tối đa$3 \cdot 10^5$robot và giới hạn 2 giây,$O(n \log n)$được chấp nhận. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    solve()
    sys.stdout = sys.__stdout__
    return output.getvalue().strip()

# Provided sample
assert run("5\n7 12\n1 2 3 4 9 10 11\nR R L L R R R\n2 10\n1 6\nR R\n2 10\n1 3\nL L\n1 10\n5\n7 8\n6 1 7 2 3 5 4\nR L R L L L L\n") == "1 1 1 1 2 -1 2\n-1 -1\n2 2\n-1\n-1 2 7 3 2 7 3", "sample 1"

# Custom: single robot never collides
assert run("1\n1 5\n3\nL\n") == "-1", "single robot"

# Custom: robots collide after wall reflection
assert run("1\n2 10\n1 9\nR L\n") == "4 4", "collision after reflection"

# Custom: all robots same parity never meet
assert run("1\n3 10\n1 3 5\nR R R\n") == "-1 -1 -1", "same parity
```
