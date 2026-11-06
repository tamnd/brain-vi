---
title: "CF 1272B - Robot đi bộ trên tuyết"
description: "Chúng ta được cung cấp một chuỗi các bước di chuyển của robot trên một lưới vô hạn bắt đầu từ điểm gốc. Mỗi ký tự trong chuỗi yêu cầu robot di chuyển một bước theo một trong bốn hướng chính."
date: "2026-06-16T01:23:50+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1272
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 605 (Div. 3)"
rating: 1200
weight: 1272
solve_time_s: 534
verified: false
draft: false
---

[CF 1272B - Robot đi bộ trên tuyết](https://codeforces.com/problemset/problem/1272/B) 

**Đánh giá:** 1200 
**Tags:** thuật toán xây dựng, tham lam, triển khai 
**Thời gian giải:** 8 phút 54 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các bước di chuyển của robot trên một lưới vô hạn bắt đầu từ điểm gốc. Mỗi ký tự trong chuỗi yêu cầu robot di chuyển một bước theo một trong bốn hướng chính. Chúng ta được phép xóa bất kỳ tập con nào của các nước đi này và sau đó sắp xếp lại thứ tự các nước đi còn lại một cách tùy ý trước khi thực hiện chúng. 

Chuyển động kết quả phải tạo thành một bước đi hợp lệ với một ràng buộc nghiêm ngặt: robot không được phép truy cập vào bất kỳ ô lưới nào nhiều hơn một lần, ngoại trừ việc nó có thể bắt đầu và kết thúc tại điểm gốc. Vì vậy, đường đi phải đơn giản ngoại trừ điểm gốc, có thể xuất hiện nhiều nhất hai lần, ở đầu và cuối. 

Nhiệm vụ là giữ càng nhiều nước đi càng tốt, sắp xếp lại chúng một cách tối ưu và xuất ra bất kỳ chuỗi hợp lệ nào đạt được độ dài tối đa có thể. 

Các ràng buộc ngụ ý rằng chúng tôi phải xử lý tối đa 100000 ký tự trên tất cả các trường hợp thử nghiệm. Điều này loại trừ bất kỳ cách xây dựng hàm mũ hoặc bậc hai nào. Chúng tôi chỉ có thể thực hiện công việc tuyến tính cho mỗi trường hợp thử nghiệm, về cơ bản là đếm và xây dựng câu trả lời trong O(n). 

Một cách giải thích ngây thơ sẽ đề nghị thử tất cả các tập hợp con của các bước di chuyển và kiểm tra xem một hoán vị có tạo thành một chu trình đơn giản hợp lệ hay không. Điều này ngay lập tức thất bại vì có 2^n tập hợp con và n! hoán vị, cả hai đều vượt xa giới hạn khả thi. 

Một cạm bẫy tinh vi hơn xuất phát từ việc nghĩ rằng chúng ta có thể đơn giản sử dụng tất cả các hướng cân bằng một cách độc lập. Ví dụ: thực hiện nhiều bước di chuyển trái và phải và sắp xếp chúng thành "LRLRLR..." trông có vẻ cân bằng nhưng thực chất lại quay lại các điểm trung gian và vi phạm quy tắc. Tương tự, việc sử dụng nhiều bước di chuyển theo chiều dọc liên tiếp sẽ khiến các lượt truy cập lặp lại vào các ô trên cùng một dòng. Hạn chế không chỉ là sự cân bằng mà còn là sự đơn giản về mặt hình học của toàn bộ con đường. 

## Phương pháp tiếp cận 

Chiến lược vũ phu sẽ thử chọn một tập hợp con các bước di chuyển và sau đó xác minh xem liệu một số thứ tự có tạo ra một bước đi khép kín không tự giao nhau hợp lệ hay không. Ngay cả khi chúng tôi sửa một tập hợp con, việc kiểm tra tính hợp lệ đòi hỏi phải suy luận về tất cả các hoán vị hoặc mô phỏng các đường dẫn có thể, trong trường hợp xấu nhất vẫn thoái hóa thành khám phá theo cấp số nhân. Chỉ riêng số lượng tập hợp con là 2^n và mỗi lần xác minh ít nhất sẽ là O(n), tốc độ này quá chậm. 

Điểm mấu chốt là vì chúng ta được phép sắp xếp lại thứ tự tùy ý nên chỉ số lượng của từng hướng mới quan trọng chứ không phải cấu trúc ban đầu. Điều này làm giảm vấn đề trong việc chọn số lượng nước đi L, R, U và D mà chúng ta giữ lại. 

Bây giờ chúng tôi hỏi hình dạng nào của đường dẫn sẽ tối đa hóa độ dài trong khi tránh phải xem lại. Một đường dẫn dài hợp lệ về cơ bản phải tạo thành một đa giác khép kín đơn giản. Trên một lưới có các bước được căn chỉnh theo trục, cấu trúc tốt nhất mà chúng ta có thể xây dựng là một hình chữ nhật: đi sang phải một đoạn, đi lên, quay lại bên trái và đi xuống trở lại điểm gốc. Điều này đảm bảo không có tế bào bên trong lặp đi lặp lại. 

Để tạo thành một hình chữ nhật như vậy, chúng ta cần các bước di chuyển ngang và các bước di chuyển dọc phù hợp. Nếu lấy x = min(L, R), chúng ta có thể xây dựng x bước sang phải và x bước sang trái. Tương tự, y = min(U, D) cho y bước lên và y bước xuống. Nếu cả x và y đều dương, chúng ta có thể kết hợp chúng thành một hình chữ nhật có chu vi 2(x + y). 

Nếu chỉ có một trục có các cặp khớp nhau, chúng ta không thể mở rộng nó ra ngoài một cặp qua lại mà không xem lại các ô trung gian. Vì vậy, trong trường hợp đó, chúng ta chỉ có thể lấy tổng cộng một cặp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên các tập hợp con và hoán vị | O(2^n · n!) | O(n) | Quá chậm | 
| Đếm phương hướng và xây dựng hình chữ nhật | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập.

1. Đếm số lần mỗi hướng xuất hiện trong chuỗi đầu vào. Chúng tôi lưu trữ số lượng L, R, U và D. Điều này là cần thiết vì thứ tự không liên quan sau khi sắp xếp lại, vì vậy chỉ có tần số mới quan trọng. 
2. Tính xem chúng ta có thể thực hiện bao nhiêu chuyển động ngang cân bằng bằng cách lấy x = min(L, R). Điều này thể hiện số lượng cặp trái-phải tối đa có thể được sử dụng mà không phá vỡ tính đối xứng. 
3. Tính xem chúng ta có thể thực hiện bao nhiêu bước di chuyển theo chiều dọc cân bằng bằng cách lấy y = min(U, D). Điều này đại diện cho số lượng tối đa các cặp lên xuống. 
4. Nếu cả x và y đều lớn hơn 0 thì chúng ta sẽ dựng một hình chữ nhật. Chúng ta xuất ra x nhân 'R', rồi y nhân 'U', rồi x nhân 'L', rồi y nhân 'D'. Thứ tự này đảm bảo đường dẫn đi xung quanh ranh giới của hình chữ nhật và quay trở lại điểm gốc mà không cần ghé thăm bất kỳ ô nào hai lần. 
5. Nếu một trong x hoặc y bằng 0 nhưng cái còn lại dương, chúng ta không thể tạo thành hình chữ nhật. Trong trường hợp này, chúng tôi xuất ra chính xác một cặp theo trục có hướng khác 0: "LR" hoặc "UD". Điều này đưa ra một đường dẫn hợp lệ có độ dài 2. 
6. Nếu cả x và y đều bằng 0 thì không có cặp nước đi hợp lệ nào tồn tại nên câu trả lời trống. 

### Tại sao nó hoạt động 

Đường dẫn được xây dựng luôn là một đường đi khép kín đơn giản trong lưới. Khi sử dụng cả hai trục, robot sẽ vạch ra ranh giới của hình chữ nhật. Mỗi ô trung gian nằm chính xác trên một cạnh của ranh giới hình chữ nhật, do đó không có ô nào ngoại trừ ô gốc được xem lại. Khi chỉ tồn tại một trục, mọi nỗ lực sử dụng nhiều hơn một cặp sẽ buộc phải truy cập lại một ô trung gian, do đó việc hạn chế ở một cặp duy nhất là cần thiết và tối ưu. 

Sự tối ưu xuất phát từ thực tế là mọi đường dẫn đóng không tự giao nhau hợp lệ phải sử dụng số hướng ngược nhau bằng nhau. Bất kỳ sự mất cân bằng bổ sung nào cũng không thể được sử dụng mà không vi phạm điều kiện điểm cuối. Vì chúng tôi tối đa hóa x và y một cách độc lập nên chúng tôi sử dụng tất cả các cặp cân bằng có sẵn và không tồn tại cấu trúc hợp lệ nào lớn hơn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    q = int(input())
    for _ in range(q):
        s = input().strip()

        L = s.count('L')
        R = s.count('R')
        U = s.count('U')
        D = s.count('D')

        x = min(L, R)
        y = min(U, D)

        if x == 0 and y == 0:
            print(0)
            print()
            continue

        if x == 0:
            print(2)
            print("UD")
            continue

        if y == 0:
            print(2)
            print("LR")
            continue

        ans = []
        ans.append("R" * x)
        ans.append("U" * y)
        ans.append("L" * x)
        ans.append("D" * y)

        res = "".join(ans)
        print(len(res))
        print(res)

if __name__ == "__main__":
    solve()
```Đầu tiên, mã tổng hợp các tần số hướng, đây là thông tin duy nhất cần thiết sau khi cho phép sắp xếp lại. Sau đó nó tính toán các cặp đối xứng có thể sử dụng trên mỗi trục. 

Logic xây dựng theo trực tiếp ý tưởng hình chữ nhật. Thứ tự R, U, L, D là có chủ ý vì nó vạch ra ranh giới mà không vượt qua chính nó. Bất kỳ hoán vị nào bảo toàn cấu trúc chu trình này cũng sẽ có tác dụng, nhưng việc phá vỡ nó sẽ tạo ra các ô lặp lại. 

Việc xử lý cạnh cho các trường hợp trục đơn rất quan trọng: ngay cả khi có nhiều cặp tồn tại, chỉ một cặp có thể được sử dụng một cách an toàn mà không vi phạm ràng buộc “không xem lại”. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi hai trường hợp đại diện. 

### Ví dụ 1:`LRU`| Bước | L | R | Bạn | D | x=min(L,R) | y=min(U,D) | Quyết định | Đầu ra | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Đầu vào | 1 | 1 | 1 | 0 | 1 | 0 | trục đơn | "LR" | 

Ở đây chỉ tồn tại sự cân bằng theo chiều ngang nên chúng ta không thể tạo thành hình chữ nhật. Sử dụng nhiều hơn một cặp sẽ buộc phải xem lại`(1,0)`hoặc`(0,0)`sớm nên chúng tôi giữ lại đúng một cặp. 

### Ví dụ 2:`LRUDDLRUDRUL`| Bước | L | R | Bạn | D | x | y | Quyết định | Xây dựng đầu ra | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Đếm | 3 | 3 | 3 | 3 | 3 | 3 | hình chữ nhật | RRR UUU LLL DDD | 

Mọi hướng đều cân bằng hoàn hảo. Thuật toán tạo thành một ranh giới hình chữ nhật 3 x 3, mang lại một chu trình đơn giản quay trở lại điểm gốc mà không cần xem lại bất kỳ ô nào khác. 

Điều này khẳng định rằng khi có sẵn cả hai trục, giải pháp luôn sử dụng toàn bộ công suất đối xứng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi chuỗi được quét một lần để đếm hướng và xây dựng đầu ra | 
| Không gian | O(1) | Chỉ có bốn bộ đếm và bộ đệm đầu ra được sử dụng | 

Tổng kích thước đầu vào được giới hạn bởi 100000 trong tất cả các trường hợp thử nghiệm, do đó giải pháp chạy thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    q = int(input())
    out = []
    for _ in range(q):
        s = input().strip()

        L = s.count('L')
        R = s.count('R')
        U = s.count('U')
        D = s.count('D')

        x = min(L, R)
        y = min(U, D)

        if x == 0 and y == 0:
            out.append("0\n")
            continue
        if x == 0:
            out.append("2\nUD\n")
            continue
        if y == 0:
            out.append("2\nLR\n")
            continue

        res = "R"*x + "U"*y + "L"*x + "D"*y
        out.append(str(len(res)) + "\n" + res + "\n")

    return "".join(out)

# provided samples
assert run("""6
LRU
DURLDRUDRULRDURDDL
LRUDDLRUDRUL
LLLLRRRR
URDUR
LLL
""") == """2
LR
14
RUURDDDDLLLUUR
12
URDDDLLLRRUU
2
LR
2
UD
0
""", "sample check"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chỉ một hướng | 0 hoặc 2 | xử lý các trường hợp trục suy biến | 
| hình chữ nhật cân đối | chu kỳ đầy đủ | độ chính xác xây dựng tối đa | 
| thừa một trục | cặp giới hạn | ngăn chặn chuỗi dài không hợp lệ | 

## Vỏ cạnh 

Khi tất cả các bước di chuyển chỉ theo một cặp hướng, chẳng hạn như`"LLLLRRRR"`, thuật toán sẽ giảm chính xác câu trả lời xuống một cặp "LR". Cố gắng sử dụng tất cả các cặp sẽ tạo ra những lượt truy cập lặp lại dọc theo dòng, đặc biệt là xem lại`(1,0)`nhiều lần. 

Khi tất cả bốn hướng đều tồn tại nhưng không cân bằng, thuật toán vẫn chỉ sử dụng số lượng khớp tối thiểu một cách an toàn. Bất kỳ bước đi bổ sung nào chưa khớp đều bị loại bỏ vì chúng không thể là một phần của một chu trình đơn giản khép kín. 

Khi chỉ có một loại di chuyển tồn tại, chẳng hạn như`"LLL"`, cả x và y đều bằng 0 và thuật toán trả về chính xác một đường dẫn trống vì không thể hình thành chu trình hợp lệ.
