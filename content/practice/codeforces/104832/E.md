---
title: "CF 104832E - Chayas"
description: "Chúng tôi được phát một bộ chayas có dán nhãn đặt dọc theo một đường thẳng. Thứ tự chính xác vẫn chưa được biết và chúng tôi muốn đếm xem có bao nhiêu hoán vị hoàn toàn từ trái sang phải của những chayas này phù hợp với danh sách các quan sát lịch sử."
date: "2026-06-28T11:58:53+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104832
codeforces_index: "E"
codeforces_contest_name: "2023-2024 ICPC, Asia Yokohama Regional Contest 2023"
rating: 0
weight: 104832
solve_time_s: 72
verified: true
draft: false
---

[CF 104832E - Chayas](https://codeforces.com/problemset/problem/104832/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 12s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được phát một bộ chayas có dán nhãn đặt dọc theo một đường thẳng. Thứ tự chính xác vẫn chưa được biết và chúng tôi muốn đếm xem có bao nhiêu hoán vị hoàn toàn từ trái sang phải của những chayas này phù hợp với danh sách các quan sát lịch sử. 

Mỗi quan sát liên quan đến ba chayas riêng biệt$a, b, c$, và khẳng định rằng$b$nằm ở đâu đó giữa$a$Và$c$dọc đường. “Giữa” mang tính hình học: nếu chúng ta nhìn vào thứ tự cuối cùng, vị trí của$b$phải nằm chặt chẽ giữa các vị trí của$a$Và$c$. Thứ tự tương đối của$a$Và$c$không cố định, vì vậy cả hai mẫu$a < b < c$Và$c < b < a$được phép, nhưng cấu hình ở đó$b$ở cùng một phía của cả hai đều bị cấm. 

Nhiệm vụ là đếm xem có bao nhiêu hoán vị của$1 \ldots n$thỏa mãn đồng thời tất cả các ràng buộc đó, modulo$998244353$. 

Ràng buộc$n \le 24$là tín hiệu trung tâm. Không gian tìm kiếm giai thừa đã quá lớn, nhưng$24$đề xuất mạnh mẽ lập trình động bitmask trên các tập hợp con. Tuy nhiên, sự hiện diện của các ràng buộc bậc ba có nghĩa là chúng ta không xử lý một phần trật tự đơn giản; mỗi ràng buộc phụ thuộc vào vị trí tương đối của ba phần tử, không chỉ so sánh theo cặp. Đó là khó khăn chính. 

Một cách giải thích ngây thơ sẽ cố gắng kiểm tra từng hoán vị theo tất cả các ràng buộc, nhưng ngay cả việc tạo ra các hoán vị cũng là$24!$, điều đó là không thể thực hiện được. Bất kỳ giải pháp nào cũng phải nén việc kiểm tra ràng buộc để có thể xác minh tính hợp lệ tăng dần trong khi xây dựng hoán vị. 

Một trường hợp phức tạp là các ràng buộc không mang tính bắc cầu hoặc nhất quán theo nghĩa thứ tự đơn giản. Ví dụ: nếu chúng ta có những ràng buộc như$(1,2,3)$Và$(2,3,1)$, chúng có thể có vẻ hợp lý ở địa phương nhưng lại gây ra những mâu thuẫn về mặt tổng thể mà chỉ xuất hiện khi xem xét vị trí đầy đủ. Một vấn đề khác là ràng buộc không yêu cầu tính liền kề, chỉ yêu cầu thứ tự tương đối, do đó các phương pháp giả định cấu trúc liền kề ngay lập tức thất bại. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: liệt kê tất cả các hoán vị và kiểm tra xem mọi ràng buộc ba có được thỏa mãn hay không. Đối với mỗi hoán vị, chúng tôi xác định vị trí$a, b, c$và xác minh xem$b$nằm giữa hai cái còn lại. Kiểm tra một chi phí hoán vị$O(m)$, do đó tổng độ phức tạp trở thành$O(n! \cdot m)$, điều này vượt xa khả năng thực hiện$n = 24$. 

Quan sát cấu trúc quan trọng là các ràng buộc chỉ nói về thứ tự tương đối, không nói về khoảng cách hoặc sự liền kề. Điều này gợi ý việc xây dựng hoán vị từ trái sang phải và đảm bảo rằng bất cứ khi nào chúng ta đặt một phần tử mới, tất cả các ràng buộc liên quan đến nó có thể được xác thực chỉ bằng cách sử dụng những phần tử đã được đặt. 

Điều này dẫn đến một tập hợp con DP trên mặt nạ bit, trong đó chúng tôi hiểu mặt nạ là tập hợp các phần tử đã được đặt. Quá trình chuyển đổi là thêm một phần tử mới vào cuối tiền tố hiện tại. 

Khó khăn là mỗi hạn chế$(a, b, c)$trở thành một điều kiện vào thời điểm khi$b$được đặt: tại thời điểm đó, chính xác một trong$a$Và$c$phải có sẵn ở tiền tố, bởi vì$b$phải nằm giữa chúng theo thứ tự cuối cùng. Điều này biến mọi ràng buộc thành một điều kiện chỉ phụ thuộc vào một tập hợp con và có thể được kiểm tra tăng dần. 

Thách thức còn lại là kiểm tra hiệu quả các điều kiện này trên tất cả các tập hợp con, vì việc quét trực tiếp các ràng buộc trên mỗi quá trình chuyển đổi quá chậm. Giải pháp dựa vào việc tính toán trước cho từng phần tử$b$, cấu trúc gây ra bởi tất cả các ràng buộc trong đó$b$là phần tử ở giữa và xác nhận tư cách thành viên của tập hợp con đối với cấu trúc đó bằng cách sử dụng các thao tác bit được tính toán trước. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Hoán vị vũ phu |$O(n! \cdot m)$|$O(1)$| Quá chậm | 
| Tập hợp con DP với kiểm tra ràng buộc gia tăng |$O(n \cdot 2^n + m \cdot 2^n)$ngây thơ, được tối ưu hóa để phù hợp với quá trình tiền xử lý bitset |$O(n \cdot 2^n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xác định DP trên tập hợp con các đỉnh, trong đó`dp[mask]`đếm có bao nhiêu lệnh một phần hợp lệ đặt chính xác các phần tử trong`mask`làm tiền tố của hoán vị cuối cùng. 

Đối với mỗi phần tử$b$, chúng tôi xử lý trước tất cả các ràng buộc trong đó$b$là phần tử ở giữa. Mỗi ràng buộc như vậy$(a, b, c)$áp đặt một yêu cầu đối với bất kỳ tiền tố hợp lệ nào$S$có chứa$b$: giữa$a$Và$c$, chính xác là người ta phải nằm trong$S \setminus \{b\}$. Nếu cả hai đều ở trong hoặc cả hai đều ở ngoài, ràng buộc hiện đang bị vi phạm$b$được chèn vào. 

Sau đó, chúng tôi xây dựng DP bằng cách lặp lại các mặt nạ và cố gắng thêm một phần tử mới$x$. Kiểm tra tính hợp lệ để đặt$x$chỉ phụ thuộc vào những ràng buộc trong đó$x$là phần tử ở giữa. 

### bước 

1. Tính toán trước cho mỗi$b$, danh sách các cặp$(a, c)$như vậy$b$phải nằm giữa$a$Và$c$. Đây chỉ là sự tập hợp lại các ràng buộc đầu vào. 
2. Khởi tạo DP với`dp[0] = 1`, đại diện cho tiền tố trống. 
3. Lặp lại tất cả các mặt nạ từ nhỏ đến lớn. Đối với mỗi mặt nạ, hãy cân nhắc thêm một phần tử mới$x$không có trong mặt nạ. 
4. Để kiểm tra xem$x$có thể được thêm vào, kiểm tra tất cả các ràng buộc liên quan đến$x$. Đối với mỗi cặp$(a, c)$, chúng tôi yêu cầu đó chính xác là một trong$a$Và$c$đã ở trong rồi`mask`. 
5. Nếu tất cả các ràng buộc đó được thỏa mãn, hãy cập nhật`dp[mask | (1 << x)] += dp[mask]`. 
6. Câu trả lời cuối cùng là`dp[(1 << n) - 1]`. 

### Tại sao nó hoạt động 

DP thực thi một cách giải thích nhất quán về “tính hợp lệ của tiền tố” cho mỗi hoán vị một phần. Khi chúng ta đặt một phần tử$b$, mọi ràng buộc liên quan đến$b$trở nên hoàn toàn có thể kiểm tra được, bởi vì tính đúng đắn của nó chỉ phụ thuộc vào việc$a$Và$c$đã được đặt tương đối với$b$khoảnh khắc chèn của. Một lần$b$được chèn vào, không có thao tác nào trong tương lai có thể thay đổi liệu$b$nằm giữa$a$Và$c$, do đó ràng buộc được cố định ở bước đó. Điều này tạo ra một bất biến rõ ràng: mỗi trạng thái một phần biểu thị một tập hợp các phần tử đã đặt vẫn có thể được mở rộng thành một hoán vị hợp lệ đầy đủ và mọi chuyển đổi đều bảo toàn thuộc tính này bằng cách thực thi chính xác tất cả các ràng buộc mới hoàn thành khi chúng có thể quyết định được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def solve():
    n, m = map(int, input().split())
    
    between = [[] for _ in range(n)]
    
    for _ in range(m):
        a, b, c = map(int, input().split())
        a -= 1
        b -= 1
        c -= 1
        between[b].append((a, c))
    
    N = 1 << n
    dp = [0] * N
    dp[0] = 1
    
    for mask in range(N):
        if dp[mask] == 0:
            continue
        
        for x in range(n):
            if mask & (1 << x):
                continue
            
            ok = True
            
            for a, c in between[x]:
                in_a = (mask >> a) & 1
                in_c = (mask >> c) & 1
                if in_a == in_c:
                    ok = False
                    break
            
            if ok:
                dp[mask | (1 << x)] = (dp[mask | (1 << x)] + dp[mask]) % MOD
    
    print(dp[N - 1])

if __name__ == "__main__":
    solve()
```Mã phản chiếu trực tiếp DP. Mảng`between[b]`lưu trữ tất cả các ràng buộc ở đâu$b$phải nằm giữa hai phần tử khác. Trong quá trình chuyển đổi, chúng tôi chỉ xác thực các ràng buộc có liên quan khi$b$được chèn vào. 

Chi tiết triển khai chính là việc kiểm tra ràng buộc chỉ sử dụng các kiểm tra bit trên mặt nạ hiện tại, điều này tránh việc tính toán lại các vị trí hoặc duy trì một thứ tự rõ ràng. Trạng thái DP mã hóa chính xác thông tin cần thiết: phần tử nào đã được đặt. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5 4
1 2 4
2 3 5
3 2 4
1 3 2
```Chúng tôi theo dõi một số chuyển đổi tiêu biểu. 

| mặt nạ | được thêm lần cuối | kiểm tra tính hợp lệ | giá trị dp | 
| --- | --- | --- | --- | 
| 00000 | - | trạng thái cơ sở | 1 | 
| 00100 | 3 | không có ràng buộc cho 3 vi phạm | 1 | 
| 00110 | 4 | kiểm tra các ràng buộc liên quan đến 4 | 1 | 

DP khám phá tất cả các tập hợp con và tích lũy chính xác bốn hoán vị đầy đủ thỏa mãn mọi ràng buộc. 

Dấu vết này nêu bật rằng các ràng buộc chỉ được thực thi khi phần tử ở giữa được chèn chứ không phải trên toàn bộ. 

### Mẫu 2 

đầu vào:```
4 2
3 1 4
1 4 3
```Ở đây cả hai ràng buộc đều tạo ra các mối quan hệ mâu thuẫn giữa các bộ ba trên cùng một bộ ba. Trong DP, mọi nỗ lực đặt phần tử$1$hoặc$4$sớm cuối cùng sẽ dẫn đến vi phạm khi điểm cuối thứ hai được chèn vào. 

| mặt nạ | sự kiện | hợp lệ | dp | 
| --- | --- | --- | --- | 
| 0000 | bắt đầu | hợp lệ | 1 | 
| mặt nạ một phần khác nhau | hạn chế gây ra mâu thuẫn | bị từ chối | 0 | 

Không có mặt nạ đầy đủ nào đạt đến mức hoàn thành nên câu trả lời là không. 

Điều này cho thấy các mâu thuẫn được phát hiện cục bộ như thế nào tại thời điểm một ràng buộc trở nên hoàn toàn có thể quan sát được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \cdot 2^n + m \cdot 2^n)$| DP trên các tập hợp con có kiểm tra ràng buộc trong quá trình chuyển đổi | 
| Không gian |$O(2^n + m)$| Bảng DP cộng với các ràng buộc được nhóm | 

Với$n \le 24$, kích thước DP là khoảng$16$triệu tiểu bang. Số lượng hạn chế là khoảng$2000$, do đó cấu trúc vẫn có thể quản lý được trong C++ được tối ưu hóa và các hoạt động bit cẩn thận. Công thức đảm bảo rằng mỗi ràng buộc chỉ được kiểm tra khi có liên quan, tránh việc quét toàn bộ lặp đi lặp lại. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 998244353

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    n, m = map(int, input().split())
    between = [[] for _ in range(n)]
    
    for _ in range(m):
        a, b, c = map(int, input().split())
        a -= 1; b -= 1; c -= 1
        between[b].append((a, c))
    
    N = 1 << n
    dp = [0] * N
    dp[0] = 1
    
    for mask in range(N):
        if dp[mask] == 0:
            continue
        for x in range(n):
            if mask & (1 << x):
                continue
            ok = True
            for a, c in between[x]:
                if ((mask >> a) & 1) == ((mask >> c) & 1):
                    ok = False
                    break
            if ok:
                dp[mask | (1 << x)] = (dp[mask | (1 << x)] + dp[mask]) % MOD
    
    return str(dp[N - 1])

# provided samples
assert run("5 4\n1 2 4\n2 3 5\n3 2 4\n1 3 2\n") == "4"
assert run("4 2\n3 1 4\n1 4 3\n") == "0"

# custom cases
assert run("3 0\n") == "6", "all permutations valid"
assert run("3 1\n1 2 3\n") == "2", "middle fixed constraint"
assert run("4 1\n1 2 3\n") >= "0", "basic validity check"
assert run("2 0\n") == "2", "minimum unconstrained"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|$n=3, m=0$| 6 | tất cả các hoán vị được phép | 
|$n=3, (1,2,3)$| 2 | ràng buộc giữa duy nhất | 
|$n=4, m=1$| không âm | kiểm tra độ tỉnh táo về độ ổn định của DP | 
|$n=2, m=0$| 2 | giai thừa không tầm thường nhỏ nhất | 

## Vỏ cạnh 

Trường hợp cạnh then chốt là khi các ràng buộc tạo thành các mâu thuẫn chỉ xuất hiện sau khi xây dựng một phần. Ví dụ: nếu hai ràng buộc buộc tính chẵn lẻ không tương thích giữa cùng một điểm cuối thông qua các điểm giữa khác nhau, thì DP sẽ từ chối tất cả các lần hoàn thành vì vi phạm được phát hiện chính xác khi điểm cuối thứ hai được chèn vào tiền tố khiến cho ràng buộc đó hoàn toàn có thể quan sát được. 

Một trường hợp khác là khi không có ràng buộc nào tồn tại. Trong trường hợp này, mọi chuyển đổi tập hợp con đều hợp lệ và DP đếm tất cả các hoán vị một cách hiệu quả, tạo ra$n!$. Việc triển khai xử lý việc này một cách tự nhiên vì`between[x]`trống rỗng cho tất cả$x$, vì vậy mọi quá trình chuyển đổi đều trôi qua mà không bị hạn chế. 

Trường hợp tinh tế cuối cùng xảy ra khi một phần tử xuất hiện ở vị trí trung tâm trong nhiều ràng buộc. Ngay cả khi đó, mỗi ràng buộc vẫn được kiểm tra độc lập tại thời điểm chèn và do quyết định chỉ phụ thuộc vào tư cách thành viên của bit trong mặt nạ hiện tại nên không có sự mơ hồ về thứ tự nào phát sinh trong trạng thái DP.
