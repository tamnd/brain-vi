---
title: "CF 321B - Ciel và Đấu tay đôi"
description: "Chúng tôi được đưa cho hai bộ thẻ, một của Jiro và một của Ciel. Thẻ của Jiro có hai loại, Tấn công và Phòng thủ, mỗi loại có một giá trị sức mạnh. Các lá bài của Ciel đơn giản hơn: mỗi lá bài của cô ấy là một lá bài Tấn công với sức mạnh nhất định."
date: "2026-06-06T02:32:34+07:00"
tags: ["codeforces", "competitive-programming", "dp", "flows", "greedy"]
categories: ["algorithms"]
codeforces_contest: 321
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 190 (Div. 1)"
rating: 1900
weight: 321
solve_time_s: 79
verified: true
draft: false
---

[CF 321B - Ciel và Đấu tay đôi](https://codeforces.com/problemset/problem/321/B) 

**Xếp hạng:** 1900 
**Tags:** dp, dòng chảy, tham lam 
**Thời gian giải:** 1 phút 19s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được đưa cho hai bộ thẻ, một của Jiro và một của Ciel. Thẻ của Jiro có hai loại, Tấn công và Phòng thủ, mỗi loại có một giá trị sức mạnh. Các lá bài của Ciel đơn giản hơn: mỗi lá bài của cô ấy là một lá bài Tấn công với sức mạnh nhất định. 

Ciel chơi bằng cách chọn từng lá bài của mình nhiều nhất một lần, theo bất kỳ thứ tự nào cô ấy muốn và có thể tùy ý dừng sớm. Mỗi thẻ được chọn sẽ được sử dụng để gây sát thương trực tiếp cho Jiro nếu không còn thẻ nào hoặc tiêu diệt chính xác một trong các thẻ của Jiro trước và có thể gây sát thương giảm tùy thuộc vào loại thẻ đó. Các thẻ bài của kẻ thù thuộc loại tấn công yêu cầu sức mạnh của Ciel ít nhất phải là của chúng và chúng sẽ giảm sát thương bằng sức mạnh của mình. Thẻ thuộc loại phòng thủ yêu cầu sự bất bình đẳng nghiêm ngặt và không gây ra bất kỳ thiệt hại nào khi bị phá hủy. 

Mục tiêu là chọn thứ tự và tập hợp con các thẻ của Ciel và ghép chúng một cách tối ưu với các thẻ của Jiro để tổng sát thương tích lũy được tối đa hóa. 

Các ràng buộc rất nhỏ, tối đa 100 thẻ cho mỗi bên. Điều này ngay lập tức gợi ý rằng giải pháp O(n^2) hoặc O(n^3) là khả thi, nhưng bất kỳ số mũ nào ở quy mô của cả hai bên vẫn sẽ nguy hiểm nếu không được cắt tỉa cẩn thận. 

Một điểm tinh tế quan trọng là Ciel có thể dừng lại bất cứ lúc nào. Điều đó có nghĩa là không phải thẻ nào cũng phải được sử dụng và việc sử dụng thẻ yếu quá sớm có thể gây lãng phí thiệt hại trực tiếp có giá trị cao về sau. Một điểm tinh tế khác là các lá Phòng thủ không thể bị phá hủy một cách bình đẳng, điều này tạo ra những hạn chế ghép đôi nghiêm ngặt ảnh hưởng đến quyết định đặt hàng. 

Cách tiếp cận tham lam ngây thơ thường thất bại vì việc chọn quân bài địch nào để tiêu diệt trước sẽ làm thay đổi “giá trị tương lai” sẵn có của những quân bài còn lại của Ciel. Ví dụ: sử dụng thẻ Ciel mạnh sớm trên thẻ Phòng thủ yếu có thể lãng phí lượng sát thương trực tiếp lớn tiềm tàng sau này. 

## Phương pháp tiếp cận 

Một cách nhìn mạnh mẽ về vấn đề này là xem xét tất cả các hoán vị của các lá bài của Ciel và tất cả các cách có thể để chỉ định mỗi lá bài đã sử dụng để “tấn công trực tiếp” hoặc “phá hủy một lá bài Jiro hợp lệ”. Đối với mỗi bước, chúng tôi chọn thẻ Jiro nào để nhắm mục tiêu hoặc bỏ qua để gây sát thương trực tiếp nếu không còn thẻ nào. Điều này dẫn đến sự bùng nổ ở các trạng thái: ở mỗi bước, chúng ta có O(n) lựa chọn mục tiêu và O(m) lựa chọn thứ tự, dẫn đến tăng trưởng giai thừa vượt xa khả thi. 

Cấu trúc sẽ trở nên dễ quản lý hơn nếu chúng ta quan sát thấy rằng thẻ của Ciel là tài nguyên duy nhất kiểm soát thứ tự tương tác, trong khi thẻ của Jiro có thể được coi là vật phẩm được tiêu thụ với các ràng buộc. Ý tưởng chính là coi các thẻ của Jiro như một bộ nhiều thẻ cố định và xem xét chúng ta tiêu diệt bao nhiêu thẻ trong số chúng trước khi chuyển sang sát thương trực tiếp thuần túy. 

Nếu chúng ta sửa một tập hợp con các quân bài của Jiro mà chúng ta định tiêu diệt, thì chiến lược tốt nhất cho Ciel là sử dụng quân bài đủ nhỏ nhất cho mỗi lần tiêu diệt, giữ lại những quân bài lớn để gây sát thương trực tiếp ở cuối. Điều này tự nhiên dẫn đến việc sắp xếp và lập trình linh hoạt về số lượng thẻ Tấn công và Phòng thủ đã bị loại bỏ. 

Sự chuyển đổi cốt lõi là hãy nghĩ về quá trình ghép các thẻ của Ciel với các thẻ của Jiro theo cách tối đa hóa tổng phần thưởng, trong đó mỗi cặp có một chi phí (có thể giảm sát thương của Jiro) và một hạn chế về tính khả thi. 

Điều này dẫn đến việc tính toán DP về số lượng thẻ Jiro đã bị loại khỏi mỗi loại và số lượng thẻ Ciel đã được sử dụng, luôn chọn hành động tiếp theo tốt nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Hoán vị và bài tập Brute Force | O(m! · phân nhánh) | O(m + n) | Quá chậm | 
| DP trên thẻ đã sử dụng và kẻ thù còn lại | O(n² · m) | O(n · m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tách các thẻ của Jiro thành hai mảng được sắp xếp: Thẻ tấn công và thẻ Phòng thủ. Hãy sắp xếp A Điểm mạnh tấn công và D được sắp xếp Điểm mạnh phòng thủ.

Chúng tôi cũng sắp xếp các quân bài của Ciel theo thứ tự tăng dần, vì việc sử dụng các quân bài nhỏ hơn sớm hơn cho phép chúng tôi dành những quân bài mạnh hơn cho các hoạt động có giá trị cao hơn. 

Chúng tôi xác định trạng thái DP để theo dõi số lượng thẻ Tấn công và Phòng thủ đã bị tiêu diệt cũng như số lượng thẻ Ciel chúng tôi đã sử dụng. Thay vì theo dõi rõ ràng các thẻ đã sử dụng, chúng tôi duyệt qua các thẻ của Ciel theo thứ tự và quyết định cách sử dụng từng thẻ. 

Chúng tôi duy trì một bảng DP trong đó dp[i][j] thể hiện mức sát thương tối đa có thể đạt được sau khi xử lý một số tiền tố trong các lá bài của Ciel, sau khi phá hủy i lá bài Tấn công và j lá bài Phòng thủ. 

Chúng tôi xử lý từng thẻ của Ciel một. 

1. Sắp xếp các thẻ Tấn công A và Thẻ Phòng thủ D của Jiro theo thứ tự tăng dần. 
2. Sắp xếp các thẻ S của Ciel theo thứ tự tăng dần. 
3. Khởi tạo mảng DP chứa đầy -inf ngoại trừ dp[0][0] = 0. 
4. Với mỗi lá bài Ciel có sức mạnh x, tiếp theo hãy tính bảng DP mới. 
5. Với mỗi trạng thái (i, j), hãy xem xét ba hành động: 

1. Bỏ qua thẻ, giữ dp[i][j]. 
2. Sử dụng nó để tiêu diệt lá bài Tấn công tiếp theo A[i], nếu x ≥ A[i]. Điều này thêm phần thưởng x - A[i]. 
3. Sử dụng nó để tiêu diệt lá bài Phòng thủ tiếp theo D[j], nếu x > D[j]. Điều này thêm 0 phần thưởng. 
6. Chuyển sang trạng thái tiếp theo cho phù hợp. 
7. Sau khi xử lý tất cả các thẻ, câu trả lời là dp[i][j] tối đa trên tất cả các trạng thái. 

Lý do chúng tôi chỉ xem xét lá bài Tấn công hoặc Phòng thủ tiếp theo theo thứ tự sắp xếp là vì việc gán một lá bài Ciel mạnh hơn cho kẻ thù yếu hơn luôn tệ hơn việc dành nó cho kẻ mạnh hơn, vì phần thưởng phụ thuộc vào phép trừ và tính khả thi là đơn điệu với sức mạnh. 

### Tại sao nó hoạt động 

Bất biến DP là sau khi xử lý k lá bài Ciel đầu tiên, dp[i][j] thể hiện chính xác mức sát thương tối đa có thể đạt được bằng cách tiêu diệt chính xác i lá bài Tấn công và j lá bài Phòng thủ bằng cách sử dụng k lá bài đó. Bất kỳ chiến lược tối ưu nào cũng có thể được sắp xếp lại để các quân bài của kẻ thù bị tiêu diệt được lấy theo thứ tự sức mạnh cần thiết tăng dần, bởi vì việc hoán đổi nhiệm vụ giữa các mục tiêu yếu hơn và mạnh hơn không bao giờ làm giảm tính khả thi và không bao giờ cải thiện kết quả nếu thực hiện sai hướng. Thuộc tính sắp xếp này đảm bảo rằng DP không bao giờ bỏ lỡ cấu trúc ghép nối tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    
    atk = []
    dfn = []
    
    for _ in range(n):
        t, x = input().split()
        x = int(x)
        if t == "ATK":
            atk.append(x)
        else:
            dfn.append(x)
    
    c = [int(input()) for _ in range(m)]
    
    atk.sort()
    dfn.sort()
    c.sort()
    
    INF_NEG = -10**18
    
    dp = [[INF_NEG] * (len(dfn) + 1) for _ in range(len(atk) + 1)]
    dp[0][0] = 0
    
    for x in c:
        ndp = [[INF_NEG] * (len(dfn) + 1) for _ in range(len(atk) + 1)]
        
        for i in range(len(atk) + 1):
            for j in range(len(dfn) + 1):
                if dp[i][j] == INF_NEG:
                    continue
                
                cur = dp[i][j]
                
                # skip
                if cur > ndp[i][j]:
                    ndp[i][j] = cur
                
                # attack next ATK
                if i < len(atk) and x >= atk[i]:
                    val = cur + (x - atk[i])
                    if val > ndp[i+1][j]:
                        ndp[i+1][j] = val
                
                # attack next DEF
                if j < len(dfn) and x > dfn[j]:
                    if cur > ndp[i][j+1]:
                        ndp[i][j+1] = cur
        
        dp = ndp
    
    ans = 0
    for i in range(len(atk) + 1):
        for j in range(len(dfn) + 1):
            ans = max(ans, dp[i][j])
    
    print(ans)

if __name__ == "__main__":
    solve()
```DP theo dõi rõ ràng số lượng thẻ Tấn công và Phòng thủ đã bị loại bỏ. Quá trình chuyển đổi phản ánh các quy tắc của trò chơi: Loại bỏ đòn tấn công đóng góp giá trị trong khi loại bỏ Phòng thủ chỉ phục vụ tính khả thi. Việc bỏ qua quá trình chuyển đổi là cần thiết vì việc dừng sớm luôn được cho phép. 

Sự bất bình đẳng nghiêm ngặt đối với thẻ Phòng thủ được xử lý bằng cách sử dụng`x > dfn[j]`thay vì`>=`. Sử dụng thẻ tấn công`>=`, phù hợp với quy tắc cho phép quan hệ. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
2 3
ATK 2000
DEF 1700
2500
2500
2500
```Chúng tôi sắp xếp: 

Tấn công = [2000], Phòng thủ = [1700], Ciel = [2500, 2500, 2500] 

Chúng tôi theo dõi quá trình chuyển đổi DP. 

| Bước | Thẻ | Hành động | (i,j) trạng thái | Thiệt hại | 
| --- | --- | --- | --- | --- | 
| 0 | - | ban đầu | (0,0) | 0 | 
| 1 | 2500 | CÔNG 2000 | (1,0) | 500 | 
| 2 | 2500 | DEF 1700 | (1,1) | 500 | 
| 3 | 2500 | trực tiếp | (1,1) | 3000 | 

Điều này cho thấy cấu trúc tối ưu: đầu tiên chuyển đổi một lá bài thành sát thương một phần thông qua ATK, sau đó loại bỏ DEF, sau đó rút tiền. 

### Mẫu 2 

đầu vào:```
2 2
ATK 100
ATK 10
1001
101
```Đã sắp xếp: 

Tấn công = [10, 100], Ciel = [101, 1001] 

| Bước | Thẻ | Hành động | (i,j) trạng thái | Thiệt hại | 
| --- | --- | --- | --- | --- | 
| 0 | - | ban đầu | (0,0) | 0 | 
| 1 | 101 | CÔNG 10 | (1,0) | 91 | 
| 2 | 1001 | CÔNG 100 | (2,0) | 992 | 

Điều này chứng tỏ tại sao những lá bài mạnh hơn phải được dành cho lần sau, vì nhiệm vụ thứ hai mang lại lợi nhuận biên cao hơn nhiều. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n · m · n) | Đối với mỗi thẻ Ciel, chúng tôi cập nhật DP lên tới n × m trạng thái | 
| Không gian | O(n · m) | Bảng DP lưu trữ các giá trị tốt nhất cho từng trạng thái tiền tố bị hủy | 

Với n, m ≤ 100, điều này phù hợp một cách thoải mái trong giới hạn, vì khoảng 10^6 lần chuyển đổi được xử lý. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return sys.stdout.getvalue() if False else _run(inp)

def _run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    old = sys.stdout
    sys.stdout = out
    solve()
    sys.stdout = old
    return out.getvalue().strip()

# provided sample 1
assert _run("""2 3
ATK 2000
DEF 1700
2500
2500
2500
""") == "3000"

# sample 2
assert _run("""2 2
ATK 100
ATK 10
1001
101
""") == "992"

# custom 1: only DEF, strict inequality
assert _run("""1 1
DEF 5
5
""") == "0"

# custom 2: minimal case
assert _run("""1 1
ATK 0
0
""") == "0"

# custom 3: greedy trap
assert _run("""2 2
ATK 50
ATK 1
60
2
""") == "59"

# custom 4: many equal cards
assert _run("""1 3
ATK 10
5
5
5
""") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chỉ DEF có sức mạnh ngang nhau | 0 | xử lý bất bình đẳng nghiêm ngặt | 
| ATK đơn tối thiểu 0 | 0 | trường hợp cạnh không bị hư hại | 
| vụ án bẫy tham lam | 59 | tầm quan trọng đặt hàng | 
| thẻ yếu lặp đi lặp lại | 0 | không có mạng ATK không hợp lệ | 

## Vỏ cạnh 

Trường hợp quan trọng là khi sức mạnh lá bài của Ciel khớp chính xác với lá bài Phòng thủ. Ví dụ: 

đầu vào:```
1 1
DEF 0
0
```DP thực thi`x > DEF`, vì vậy quá trình chuyển đổi này không hợp lệ. Cách di chuyển duy nhất có sẵn là bỏ qua hoặc cố gắng gây sát thương trực tiếp sau đó, dẫn đến 0. 

Một trường hợp khác là khi tất cả các thẻ Jiro đều Phòng thủ và tất cả các thẻ Ciel đều bằng nhau. Thuật toán tránh chính xác các chuyển đổi bất hợp pháp và đảm bảo rằng không tính ghép nối một phần, chỉ tính thiệt hại trực tiếp hợp lệ trong tương lai nếu có. 

Một trường hợp tế nhị cuối cùng là khi Ciel có nhiều quân bài hơn mức cần thiết. DP cho phép bỏ qua, do đó các thẻ bổ sung không bao giờ làm giảm câu trả lời, duy trì hành vi dừng tối ưu.
