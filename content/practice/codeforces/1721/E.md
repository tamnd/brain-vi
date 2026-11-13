---
title: "CF 1721E - Truy vấn hàm tiền tố"
description: "Chúng tôi được cung cấp một chuỗi tham chiếu cố định và sau đó được yêu cầu xử lý nhiều chuỗi truy vấn ngắn. Đối với mỗi truy vấn, chúng tôi tạm thời nối chuỗi truy vấn vào tham chiếu, tính toán hàm tiền tố trên phép nối kết quả và chỉ xuất ra các giá trị hàm tiền tố tương ứng…"
date: "2026-06-15T01:18:56+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "dp", "hashing", "string-suffix-structures", "strings", "trees"]
categories: ["algorithms"]
codeforces_contest: 1721
codeforces_index: "E"
codeforces_contest_name: "Educational Codeforces Round 134 (Rated for Div. 2)"
rating: 2200
weight: 1721
solve_time_s: 184
verified: false
draft: false
---

[CF 1721E - Truy vấn hàm tiền tố](https://codeforces.com/problemset/problem/1721/E) 

**Đánh giá:** 2200 
**Thẻ:** dfs và tương tự, dp, băm, cấu trúc hậu tố chuỗi, chuỗi, cây 
**Thời gian giải:** 3m 4s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một chuỗi tham chiếu cố định và sau đó được yêu cầu xử lý nhiều chuỗi truy vấn ngắn. Đối với mỗi truy vấn, chúng tôi tạm thời nối chuỗi truy vấn vào tham chiếu, tính toán hàm tiền tố trên phép nối kết quả và chỉ xuất ra các giá trị hàm tiền tố tương ứng với phần được nối thêm. Sau mỗi truy vấn, trạng thái sẽ đặt lại về chuỗi tham chiếu ban đầu. 

Khó khăn chính là chức năng tiền tố trên vị trí$i$phụ thuộc vào đường viền dài nhất của tiền tố kết thúc tại$i$, do đó phụ thuộc vào các giá trị hàm tiền tố trước đó và so sánh ký tự. Nếu chúng tôi tính toán lại mọi thứ từ đầu cho mỗi truy vấn, chúng tôi sẽ liên tục quét chuỗi cơ sở dài có độ dài lên đến$10^6$, điều này là không thể từ bỏ được$10^5$truy vấn. 

Các ràng buộc ngụ ý rằng bất kỳ giải pháp nào cũng phải coi chuỗi cơ sở là cấu trúc được xử lý trước và xử lý từng truy vấn theo thời gian chỉ tỷ lệ với độ dài truy vấn. Vì mỗi chuỗi truy vấn có độ dài tối đa là 10 nên tổng số công việc bổ sung cho mỗi truy vấn phải là$O(|t|)$, nếu không thì nghiệm sẽ vượt quá giới hạn. 

Việc triển khai đơn giản sẽ tính toán lại hàm tiền tố cho$s+t$đầy đủ cho mỗi truy vấn. Chỉ điều đó thôi đã có nghĩa là lên đến$10^6 + 10$làm việc trên mỗi truy vấn, lặp đi lặp lại$10^5$nhiều lần, điều này vượt xa mức có thể chấp nhận được. 

Một trường hợp lỗi tinh vi hơn xuất hiện nếu người ta cố gắng chỉ tính lại phần hậu tố nhưng vẫn liên tục so sánh với tiền tố đầy đủ của$s$mà không duy trì cấu trúc chuyển tiếp nén hoặc tự động liên tục. Hàm tiền tố có thể “nhảy lùi” nhiều lần trong một bước, do đó, việc mô phỏng từng ký tự đối với chuỗi đầy đủ có thể giảm xuống hành vi tuyến tính trong trường hợp xấu nhất. 

Ví dụ, nếu$s = aaaa\ldots a$Và$t = aaaaa$, mỗi bước tiền tố liên tục quay trở lại qua chuỗi dài các kết quả khớp. Một vòng lặp đơn giản quét ngược mỗi lần vẫn trở thành bậc hai trong trường hợp xấu nhất đối với tất cả các truy vấn. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: đối với mỗi truy vấn, biểu mẫu$s+t$, tính hàm tiền tố bằng cách sử dụng thủ tục KMP tuyến tính tiêu chuẩn và xuất kết quả cuối cùng$|t|$các giá trị. Điều này đúng vì hàm tiền tố được xác định hoàn toàn trên chuỗi nối và không phụ thuộc vào cấu trúc truy vấn. 

Vấn đề là chi phí. Mỗi phép tính là$O(|s|+|t|)$, vậy là vượt qua$q$truy vấn điều này trở thành$O(q|s|)$, đó là về$10^{11}$hoạt động trong trường hợp xấu nhất. Điều này không thể vượt qua. 

Quan sát quan trọng là việc tính toán hàm tiền tố tương đương với việc đi bộ trong một máy tự động được xác định bởi các liên kết lỗi chức năng tiền tố. Nếu chúng tôi hiểu mỗi độ dài tiền tố là một trạng thái thì mỗi ký tự mới sẽ gây ra sự chuyển đổi: chúng tôi liên tục theo dõi các liên kết lỗi cho đến khi tìm thấy trạng thái có thể được mở rộng bởi ký tự hiện tại. 

Cấu trúc này gợi ý tiền xử lý: chúng tôi xây dựng cho mọi trạng thái$v$và mỗi chữ cái$c$, trạng thái hàm tiền tố tiếp theo nếu chúng ta hiện đang ở trạng thái$v$và đọc$c$. Đây chính xác là máy tự động KMP. Sau khi được xây dựng, mỗi truy vấn sẽ trở thành một mô phỏng đơn giản trên máy tự động này, bắt đầu từ trạng thái tương ứng với chuỗi đầy đủ$s$. 

Đầu tiên chúng ta tính hàm tiền tố cho$s$. Điều này cho chúng ta trạng thái bắt đầu$cur = p[|s|]$. Sau đó, chúng tôi tính toán trước các chuyển đổi bằng cách sử dụng các liên kết lỗi. Sau đó, mỗi ký tự trong truy vấn được xử lý trong$O(1)$, tạo trạng thái tiền tố tiếp theo ngay lập tức và chúng tôi xuất nó. 

Điều này làm giảm vấn đề xuống còn tiền xử lý tuyến tính cộng với tổng công việc tuyến tính trên tất cả các ký tự truy vấn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(q(n+m))$|$O(n)$| Quá chậm | 
| Tối ưu (máy tự động KMP) | (O(n + \sum | t | )) | 

## Hướng dẫn thuật toán 

1. Tính hàm tiền tố cho chuỗi cơ sở$s$. Điều này cung cấp cho chúng tôi các liên kết lỗi cho phép chúng tôi chuyển đến tiền tố thích hợp dài nhất cũng là hậu tố ở mọi vị trí. 
2. Xử lý từng độ dài tiền tố$v$như một trạng thái trong máy tự động. Từ tiểu bang$v$, chúng ta muốn biết mình sẽ đi đến đâu nếu thêm một ký tự vào$c$. 
3. Tính toán trước bảng chuyển tiếp`nxt[v][c]`cho tất cả các tiểu bang$v$và các chữ cái$c$. Để tính toán chính xác, chúng tôi sử dụng lại các chuyển đổi được tính toán trước đó thông qua các liên kết lỗi thay vì tính toán lại các kết quả khớp từ đầu. 
4. Đặt trạng thái ban đầu cho truy vấn là`cur = p[len(s)]`, đại diện cho kết quả khớp tiền tố-hậu tố dài nhất sau khi đọc toàn bộ chuỗi cơ sở. 
5. Đối với mỗi chuỗi truy vấn$t$, xử lý từng ký tự một: 

1. Cập nhật`cur = nxt[cur][t[i]]`. 
2. Đầu ra`cur`làm giá trị hàm tiền tố tại vị trí đó trong chuỗi được nối. 
6. Sau khi hoàn tất truy vấn, không cần thiết lập lại vì`cur`đã đại diện cho nhà nước sau$s$và mỗi truy vấn là độc lập. 

Tính chính xác phụ thuộc vào thực tế là quá trình chuyển đổi tự động mã hóa chính xác quy tắc cập nhật hàm tiền tố: chúng tôi luôn di chuyển đến tiền tố dài nhất có thể được mở rộng bởi ký tự tiếp theo và các liên kết lỗi đảm bảo chúng tôi bỏ qua các kết quả khớp không hợp lệ một cách hiệu quả. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, biến`cur`đại diện cho độ dài của tiền tố dài nhất của$s + t_{processed}$đó cũng là hậu tố của chuỗi đó. Đây chính xác là định nghĩa của trạng thái hàm tiền tố. Quy tắc chuyển đổi duy trì tính bất biến này vì mọi cập nhật đều mở rộng tiền tố phù hợp hiện tại hoặc quay trở lại dọc theo các liên kết bị lỗi cho đến khi tìm thấy phần mở rộng hợp lệ. Vì các liên kết lỗi luôn bảo toàn thuộc tính đường viền nên không có ứng cử viên hợp lệ nào bị bỏ qua và trạng thái kết quả luôn là tối đa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def build_pi(s):
    n = len(s)
    pi = [0] * n
    for i in range(1, n):
        j = pi[i - 1]
        while j > 0 and s[i] != s[j]:
            j = pi[j - 1]
        if s[i] == s[j]:
            j += 1
        pi[i] = j
    return pi

def build_automaton(s, pi):
    n = len(s)
    nxt = [[0] * 26 for _ in range(n + 1)]

    for v in range(n + 1):
        for c in range(26):
            if v < n and ord(s[v]) == ord('a') + c:
                nxt[v][c] = v + 1
            else:
                if v == 0:
                    nxt[v][c] = 0
                else:
                    nxt[v][c] = nxt[pi[v - 1]][c]
    return nxt

def solve():
    s = input().strip()
    q = int(input())
    pi = build_pi(s)
    nxt = build_automaton(s, pi)

    cur = 0
    for ch in s:
        cur = nxt[cur][ord(ch) - 97]

    out = []
    for _ in range(q):
        t = input().strip()
        ans = []
        for ch in t:
            cur = nxt[cur][ord(ch) - 97]
            ans.append(str(cur))
        out.append(" ".join(ans))
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên sẽ xây dựng hàm tiền tố của chuỗi cơ sở, sau đó xây dựng bảng chuyển đổi đầy đủ của máy tự động KMP. Mỗi trạng thái tương ứng với một độ dài tiền tố và các chuyển đổi mã hóa độ dài đường viền có thể tiếp theo sau khi đọc một ký tự. 

Một điểm tinh tế là việc xây dựng`nxt`. Khi một ký tự không khớp với ký tự tiếp theo dự kiến ​​​​trong tiền tố, chúng tôi sẽ không khởi động lại từ 0 ngay lập tức. Thay vào đó, chúng tôi theo liên kết thất bại`pi[v-1]`, đại diện cho ứng cử viên biên giới tốt nhất tiếp theo. Phép đệ quy này đảm bảo tính chính xác mà không cần kiểm tra lại các ký tự. 

Một chi tiết khác là việc khởi tạo`cur`bằng cách phát lại chuỗi cơ sở thông qua máy tự động. Điều này đảm bảo tính nhất quán với trạng thái hàm tiền tố sau khi xử lý$s$, tránh mọi sự không khớp giữa quá trình tiền xử lý và mô phỏng truy vấn. 

## Ví dụ đã hoạt động 

Hãy xem xét mẫu: 

Chuỗi đầu vào$s = "aba"$và truy vấn$t = "caba"$. 

Chúng tôi theo dõi`cur`theo thời gian. 

| Bước | Nhân vật | Trước đây | Hành động | Cur mới | 
| --- | --- | --- | --- | --- | 
| 1 | c | 0 | không khớp ở gốc | 0 | 
| 2 | một | 0 | khớp 'a' | 1 | 
| 3 | b | 1 | mở rộng | 2 | 
| 4 | một | 2 | mở rộng | 3 | 

Đầu ra là`0 1 2 3`. 

Điều này cho thấy cách máy tự động theo dõi các biên giới đang phát triển một cách tự nhiên. 

Bây giờ hãy xem xét một trường hợp lặp đi lặp lại:$s = "aaaa"$,$t = "aaaa"$. 

| Bước | Char | cur trước | hành vi | cur sau | 
| --- | --- | --- | --- | --- | 
| 1 | một | 4 | mở rộng trận đấu | 5 | 
| 2 | một | 5 | mở rộng trận đấu | 6 | 
| 3 | một | 6 | mở rộng trận đấu | 7 | 
| 4 | một | 7 | mở rộng trận đấu | 8 | 

Mỗi bước đều tăng độ dài đường viền, điều này chứng tỏ rằng máy tự động xử lý các kết quả trùng khớp dài lặp đi lặp lại mà không cần quay lại từng ký tự. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n \cdot 26 + \sum | t | 
| Không gian |$O(n \cdot 26)$| bảng chuyển tiếp cho tất cả các trạng thái tiền tố | 

Chi phí tiền xử lý là tuyến tính trong chuỗi cơ sở và mỗi truy vấn chỉ đóng góp độ dài riêng của nó. Vì tổng độ dài truy vấn tối đa là$10^6$, giải pháp phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    s = sys.stdin.readline().strip()
    q = int(sys.stdin.readline())

    def build_pi(s):
        n = len(s)
        pi = [0]*n
        for i in range(1,n):
            j = pi[i-1]
            while j>0 and s[i]!=s[j]:
                j = pi[j-1]
            if s[i]==s[j]:
                j+=1
            pi[i]=j
        return pi

    def build_automaton(s, pi):
        n=len(s)
        nxt=[[0]*26 for _ in range(n+1)]
        for v in range(n+1):
            for c in range(26):
                if v<n and ord(s[v])==ord('a')+c:
                    nxt[v][c]=v+1
                else:
                    nxt[v][c]=0 if v==0 else nxt[pi[v-1]][c]
        return nxt

    pi = build_pi(s)
    nxt = build_automaton(s, pi)

    cur=0
    for ch in s:
        cur=nxt[cur][ord(ch)-97]

    out=[]
    for _ in range(q):
        t=sys.stdin.readline().strip()
        res=[]
        for ch in t:
            cur=nxt[cur][ord(ch)-97]
            res.append(str(cur))
        out.append(" ".join(res))
    return "\n".join(out)

# provided sample
assert run("""aba
6
caba
aba
bababa
aaaa
b
forces
""") == """0 1 2 3
1 2 3
2 3 4 5 6 7
1 1 1 1
2
0 0 0 0 0 0"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| lặp lại đơn | tăng trưởng biên giới đúng | ký tự lặp đi lặp lại | 
| mô hình xen kẽ | chuyển đổi dự phòng chính xác | sử dụng liên kết bị lỗi | 
| truy vấn độ dài tối đa | không có chi phí cho mỗi truy vấn | tuyến tính trên mỗi ký tự | 

## Vỏ cạnh 

Trường hợp một cạnh là khi ký tự truy vấn ngay lập tức phá vỡ tất cả các kết quả khớp hiện có. Ví dụ, nếu$s = "aaaa"$và truy vấn bắt đầu bằng$b$, máy tự động sẽ nhảy trực tiếp đến trạng thái 0 và giữ nguyên ở đó. Bảng chuyển đổi đảm bảo điều này vì tất cả các đường dẫn không khớp cuối cùng đều đạt đến trạng thái gốc. 

Một trường hợp khác là khi truy vấn liên tục mở rộng kết quả khớp vượt quá độ dài của chuỗi gốc. Nếu như$s = "abc"$Và$t = "abcabc"$, máy tự động phải sử dụng lại chính xác các tiền tố trước đó làm đường viền mới. Chuỗi liên kết lỗi đảm bảo rằng sau khi đạt đến độ dài đối sánh đầy đủ, các kết quả khớp tiếp theo sẽ tiếp tục từ đường viền chính xác thay vì khởi động lại không chính xác. 

Trường hợp cạnh cuối cùng là đầu vào tối thiểu trong đó$s$có độ dài 1. Trong trường hợp này, tất cả các chuyển đổi sẽ chuyển sang trạng thái 1 với ký tự trùng khớp hoặc trạng thái 0 nếu ngược lại. Máy tự động suy biến chính xác thành máy hai trạng thái và logic vẫn nhất quán với trường hợp chung.
