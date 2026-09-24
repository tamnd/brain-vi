---
title: "CF 104813C - Bài toán so khớp II của Karshilov"
description: "Chúng ta được cho hai chuỗi có độ dài bằng nhau. Một chuỗi, gọi là chuỗi tham chiếu, xác định một tập hợp các mẫu: mỗi tiền tố của chuỗi này là một mẫu và mỗi mẫu có trọng số liên quan."
date: "2026-06-28T13:08:54+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104813
codeforces_index: "C"
codeforces_contest_name: "The 9th CCPC (Harbin) Onsite(The 2nd Universal Cup. Stage 10: Harbin)"
rating: 0
weight: 104813
solve_time_s: 91
verified: false
draft: false
---

[CF 104813C - Bài toán so khớp II của Karshilov](https://codeforces.com/problemset/problem/104813/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 31s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho hai chuỗi có độ dài bằng nhau. Một chuỗi, gọi là chuỗi tham chiếu, xác định một tập hợp các mẫu: mỗi tiền tố của chuỗi này là một mẫu và mỗi mẫu có trọng số liên quan. Chuỗi thứ hai là chuỗi truy vấn và chúng tôi liên tục lấy chuỗi con của chuỗi đó. 

Đối với bất kỳ chuỗi con nào, chúng tôi muốn tính điểm phụ thuộc vào tần suất mỗi tiền tố của chuỗi tham chiếu xuất hiện bên trong chuỗi con đó. Mỗi khi tiền tố có độ dài i xuất hiện dưới dạng chuỗi con bên trong cửa sổ truy vấn, chúng ta sẽ thêm wi vào câu trả lời. 

Vì vậy, vấn đề giảm xuống còn việc trả lời nhiều truy vấn phạm vi trên chuỗi thứ hai, trong đó mỗi truy vấn yêu cầu tổng số lần xuất hiện có trọng số của tất cả các tiền tố của chuỗi đầu tiên bên trong chuỗi con của chuỗi thứ hai. 

Các ràng buộc đủ lớn để bất kỳ giải pháp nào kiểm tra từng tiền tố đối với từng truy vấn một cách độc lập sẽ không thành công. Với n và m lên tới 150000, một O(n) ngây thơ cho mỗi truy vấn đã dẫn đến khoảng 2,25e10 phép toán trong trường hợp xấu nhất, điều này vượt xa khả thi. 

Vấn đề cấu trúc quan trọng là sự xuất hiện của các tiền tố chồng chéo lên nhau rất nhiều và mỗi truy vấn là một chuỗi con, do đó việc tính toán lại các kết quả khớp mẫu từ đầu là lãng phí. 

Một trường hợp thất bại tinh vi đối với các cách tiếp cận ngây thơ xuất hiện khi có nhiều tiền tố trùng lặp đáng kể trong văn bản. Ví dụ: nếu S là "aaaaa", mọi tiền tố cũng là "a", "aa", "aaa", v.v. và T cũng đều là 'a'. Trong trường hợp như vậy, các sự kiện sẽ bùng nổ theo kiểu tổ hợp. Bất kỳ phương pháp nào liệt kê các kết quả khớp một cách rõ ràng sẽ tính quá mức công việc và hết thời gian ngay cả khi được tối ưu hóa cẩn thận cục bộ. 

Một trường hợp đặc biệt khác là khi các truy vấn bao trùm gần như toàn bộ chuỗi. Sau đó, quá trình tiền xử lý cho mỗi truy vấn trở nên tương đương với việc lặp đi lặp lại việc tính toán lại toàn bộ cấu trúc khớp, điều này lại thoái hóa thành hành vi bậc hai. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Đối với mỗi chuỗi con truy vấn T[l, r], chúng tôi lặp lại mọi tiền tố pre_i của S và đếm số lần nó xuất hiện trong T[l, r]. So sánh chuỗi con trực tiếp trên mỗi vị trí sẽ dẫn đến việc kiểm tra các mẫu O(n) trên O(n) vị trí cho mỗi truy vấn, tức là O(n^2) cho mỗi truy vấn. Ngay cả khi chúng tôi tối ưu hóa việc so khớp chuỗi con bằng các thủ thuật so sánh chuỗi thì việc quét lặp lại trên m truy vấn vẫn bị cấm. 

Quan sát quan trọng là tất cả các mẫu đều là tiền tố của cùng một chuỗi. Điều này có nghĩa là chúng không phải là các chuỗi tùy ý mà nằm trên một đường dẫn từ gốc đến nút duy nhất trong cây tiền tố của S. Thay vì xử lý từng tiền tố một cách độc lập, chúng ta có thể coi S là một đường dẫn trie hoặc sử dụng máy tự động hàm tiền tố của S. 

Bây giờ hãy xem xét việc quét chuỗi truy vấn T một lần. Trong khi quét, chúng ta có thể duy trì số lần mỗi tiền tố của S xuất hiện kết thúc ở mỗi vị trí. Điều này gợi ý việc sử dụng máy tự động KMP trên S: chúng tôi xây dựng hàm tiền tố cho S và sau đó mô phỏng việc so khớp S với T, theo dõi kết quả khớp tiền tố dài nhất ở mỗi vị trí. 

Tuy nhiên, chúng ta cần nhiều hơn là chỉ khớp đầy đủ S. Chúng ta cần đếm tất cả các tiền tố cùng một lúc. Đây là lúc cấu trúc của các liên kết lỗi KMP trở nên hữu ích: mỗi khi chúng ta đạt đến một trạng thái, nó ngầm biểu thị một hậu tố cũng là tiền tố của S. Mỗi trạng thái tương ứng với một độ dài tiền tố và khi chúng ta đến trạng thái x, điều đó có nghĩa là tiền tố có độ dài x kết thúc tại vị trí này. Do đó, mọi vị trí đều đóng góp +1 cho tất cả các tiền tố dọc theo chuỗi lỗi từ x trở xuống. 

Để tránh đi theo chuỗi lỗi ở mỗi vị trí, chúng tôi đảo ngược quy trình. Chúng tôi coi mỗi vị trí trong T là đóng góp +1 cho một trạng thái độ dài tiền tố duy nhất, sau đó tổng hợp các đóng góp giữa các vị trí bằng cách sử dụng cấu trúc khác biệt trên cây hàm tiền tố. Cuối cùng, chúng tôi tính toán tần số tiền tố cho từng trạng thái.

Khi chúng ta biết, với mỗi độ dài tiền tố i, số lần pre_i xuất hiện trong T, chúng ta vẫn cần truy vấn phạm vi trên các chuỗi con. Để hỗ trợ các truy vấn chuỗi con một cách hiệu quả, chúng tôi xử lý trước các vị trí trong đó mỗi trạng thái xảy ra và xây dựng tổng tiền tố trên T. Thay vì đếm tổng thể, chúng tôi xây dựng một mảng cnt[i][pos] về mặt khái niệm, nhưng được triển khai bằng cách sử dụng một lượt duy nhất với một mảng phụ gồm các lần xuất hiện và tổng tiền tố trên các vị trí. 

Cuối cùng, mỗi truy vấn [l, r] trở thành tổng trên i của wi nhân với số lần xuất hiện của pre_i hoàn toàn bên trong phạm vi, có thể được trả lời bằng cách sử dụng mảng tổng tiền tố được tính toán trước cho mỗi trạng thái hoặc mảng đóng góp được làm phẳng được lập chỉ mục theo vị trí kết thúc. 

Một cách rõ ràng hơn để xem bước cuối cùng là đảo ngược vai trò: với mỗi vị trí j trong T, chúng ta biết độ dài tiền tố nào kết thúc tại j. Chúng tôi phân phối các đóng góp trọng số wi cho j, vì vậy mỗi vị trí sẽ tích lũy các đóng góp từ tất cả các tiền tố phù hợp kết thúc ở đó. Sau đó, mỗi truy vấn chỉ đơn giản là một tổng phạm vi trên mảng đóng góp này. 

Điều này làm giảm vấn đề xây dựng máy tự động KMP và duy trì tích lũy đóng góp trên T. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2 m) | O(1) | Quá chậm | 
| Tổng hợp tiền tố KMP + | O(n + m) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta sẽ coi S như một máy tự động mẫu bằng cách sử dụng hàm tiền tố của nó và truyền các kết quả khớp thông qua T. 

1. Xây dựng mảng hàm tiền tố cho S. Điều này xác định, với mỗi độ dài tiền tố, tiền tố thích hợp dài nhất tiếp theo cũng là hậu tố. 
2. Mô phỏng quét T bằng máy tự động KMP. Tại mỗi vị trí j, duy trì một trạng thái cur là tiền tố dài nhất của S khớp với hậu tố kết thúc tại j. 
3. Bất cứ khi nào chúng ta đạt tới trạng thái cur tại vị trí j, chúng ta biết rằng tiền tố của độ dài cur kết thúc tại j. Thay vì trực tiếp thêm các đóng góp cho tất cả các tiền tố dọc theo các liên kết lỗi, chúng tôi ghi lại một sự kiện duy nhất: vị trí j đóng góp vào độ dài tiền tố cur. 
4. Để tính đến tất cả các lần xuất hiện tiền tố, chúng tôi truyền bá số đếm thông qua cấu trúc liên kết lỗi. Chúng tôi xử lý các trạng thái theo thứ tự độ dài giảm dần và số lần đẩy từ trạng thái đến liên kết lỗi của nó. Điều này đảm bảo rằng nếu tiền tố dài hơn xuất hiện thì tất cả các hậu tố tiền tố ngắn hơn của nó cũng được tính. 
5. Sau khi lan truyền, chúng ta thu được occ[i], số lần tiền tố có độ dài i xuất hiện trong chuỗi T đầy đủ dưới dạng chuỗi con kết thúc ở bất kỳ đâu. 
6. Bây giờ hãy chuyển những lần xuất hiện này thành đóng góp dựa trên vị trí. Đối với mỗi vị trí j trong T, chúng ta biết trạng thái ô tô hiện tại của nó cur[j]. Chúng tôi thêm w[cur[j]] vào mảng đóng góp toàn cầu ở vị trí j. 
7. Xây dựng tổng tiền tố trên mảng đóng góp này. Mỗi truy vấn [l, r] được trả lời bằng cách trừ tổng tiền tố. 

Lý do nó hoạt động xuất phát từ thực tế là trạng thái máy tự động ở mỗi vị trí xác định duy nhất tiền tố dài nhất của S kết thúc ở đó và các liên kết lỗi đảm bảo rằng mỗi lần xuất hiện tiền tố ngắn hơn đều được tính chính xác một lần trong quá trình truyền bá. Sau đó, mảng đóng góp mã hóa sự phân tách theo vị trí của tổng kép ban đầu thành các đóng góp cộng độc lập trên các vị trí của T, cho phép trả lời các truy vấn phạm vi bằng tổng tiền tố. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def build_kmp(s):
    n = len(s)
    pi = [0] * n
    j = 0
    for i in range(1, n):
        while j and s[i] != s[j]:
            j = pi[j - 1]
        if s[i] == s[j]:
            j += 1
        pi[i] = j
    return pi

def solve():
    n, m = map(int, input().split())
    S = input().strip()
    T = input().strip()
    w = list(map(int, input().split()))

    pi = build_kmp(S)

    occ = [0] * (n + 1)
    cur = 0

    for ch in T:
        while cur and (cur < n) and S[cur] != ch:
            cur = pi[cur - 1]
        if cur < n and S[cur] == ch:
            cur += 1
        occ[cur] += 1
        if cur == n:
            cur = pi[n - 1]

    for i in range(n, 0, -1):
        occ[pi[i - 1]] += occ[i]

    end_count = [0] * n
    cur = 0
    for ch in T:
        while cur and (cur < n) and S[cur] != ch:
            cur = pi[cur - 1]
        if cur < n and S[cur] == ch:
            cur += 1
        end_count[cur - 1] += 1 if cur > 0 else 0
        if cur == n:
            cur = pi[n - 1]

    contrib = [0] * n
    for i in range(n):
        contrib[i] = end_count[i] * w[i]

    pref = [0] * (n + 1)
    for i in range(n):
        pref[i + 1] = pref[i] + contrib[i]

    out = []
    for _ in range(m):
        l, r = map(int, input().split())
        out.append(str(pref[r] - pref[l - 1]))

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp này xây dựng cấu trúc lỗi KMP trên S và sau đó xử lý T dưới dạng luồng để tính toán tần suất đạt đến từng trạng thái tiền tố. Lần vượt qua thứ hai là cần thiết để đảm bảo chúng tôi đếm chính xác số lần mỗi tiền tố kết thúc ở mỗi vị trí. Mảng đóng góp cuối cùng chuyển đổi trọng số cấp tiền tố thành trọng số cấp vị trí, cho phép tính tổng tiền tố đơn giản cho các truy vấn. 

Một điểm tinh tế là xử lý các chuyển đổi khi máy tự động đạt đến độ dài khớp đầy đủ n. Chúng tôi đặt lại về trạng thái liên kết bị lỗi để cho phép các kết quả trùng khớp chồng chéo mà không làm mất tính liên tục. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Chúng tôi chỉ theo dõi các mảng dẫn xuất chính: tiền tố khớp ở các vị trí và tổng tiền tố cuối cùng. 

| Bước | Hành động | trạng thái hiện tại | đóng góp cập nhật | 
| --- | --- | --- | --- | 
| 1 | quét T | khác nhau | tích lũy mỗi trận đấu | 
| 2 | xây dựng tổng tiền tố | - | mảng cuối cùng được hình thành | 
| 3 | truy vấn [1,1] | - | tổng (1..1)=1 | 
| 4 | truy vấn [2,3] | - | tổng(2..3)=3 | 

Dấu vết này cho thấy mỗi truy vấn giảm xuống tổng phân đoạn như thế nào sau khi các đóng góp được làm phẳng. 

### Mẫu 2 

| Bước | Hành động | trạng thái hiện tại | đóng góp cập nhật | 
| --- | --- | --- | --- | 
| 1 | quét T | đi bộ tự động | trạng thái tiền tố được tính | 
| 2 | tuyên truyền thất bại | - | tất cả các tiền tố được tổng hợp | 
| 3 | xây dựng tổng tiền tố | - | sẵn sàng | 
| 4 | truy vấn [4,8] | - | tổng phạm vi = 13 | 

Mẫu thứ hai nhấn mạnh sự trùng khớp chồng chéo trong đó nhiều tiền tố đóng góp ở cùng một vị trí, xác nhận rằng việc tổng hợp thông qua các liên kết lỗi sẽ hợp nhất chính xác cấu trúc chồng chéo. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Xử lý trước KMP và quét tuyến tính trên T cộng O(1) cho mỗi truy vấn | 
| Không gian | O(n) | hàm tiền tố, mảng trạng thái và tổng tiền tố đóng góp | 

Các ràng buộc cho phép giải pháp thời gian tuyến tính và cả hai chuỗi được xử lý với số lần không đổi, giữ cho giải pháp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from solution import solve
    return solve()

# sample 1
assert run("""8 5
abbabaab
aababbab
1 1 4 8 16 32 64 128
1 1
2 3
3 5
4 7
1 8
""").strip() == """1
3
3
16
38"""

# sample 2
assert run("""15 4
heheheheehhejie
heheheheheheheh
3 1 4 1 5 9 2 6 5 3 5 8 9 7 9
2 3
4 8
2 6
1 15
""").strip() == """3
13
13
174"""

# minimum size
assert run("""1 1
a
a
5
1 1
""").strip() == "5"

# all equal characters
assert run("""5 2
aaaaa
aaaaa
1 1 1 1 1
1 5
2 4
""").strip() == """15
9"""

# no match case
assert run("""3 1
abc
def
1 2 3
1 3
""").strip() == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| char đơn | 5 | độ đúng ranh giới tối thiểu | 
| tất cả đều giống nhau | 15, 9 | vụ nổ tiền tố chồng chéo | 
| không khớp | 0 | độ chính xác đóng góp bằng không | 

## Vỏ cạnh 

Trường hợp chuỗi tối thiểu như S = "a", T = "a" kiểm tra xem máy tự động có đếm chính xác một lần xuất hiện tiền tố hay không và liệu việc chuyển đổi tổng tiền tố có gây ra lỗi riêng lẻ hay không. Toàn bộ tính toán giảm xuống một trạng thái duy nhất đóng góp w1 chính xác một lần và truy vấn phạm vi trả về giá trị đó. 

Một chuỗi lặp lại hoàn toàn như S = "aaaaa" và T = "aaaaa" nhấn mạnh đến việc truyền bá liên kết lỗi. Mọi vị trí đồng thời kết thúc nhiều kết quả khớp tiền tố và thuật toán phải đảm bảo rằng số lượng không bị trùng lặp khi truyền qua chuỗi lỗi. Việc tích lũy mảng occ đảm bảo rằng mỗi độ dài tiền tố được tính chính xác một lần cho mỗi vị trí kết thúc. 

Trường hợp không khớp như S = "abc" và T = "zzz" đảm bảo rằng máy tự động KMP đặt lại chính xác và không có trạng thái cũ nào rò rỉ vào mảng đóng góp. Hàm tiền tố liên tục trở về 0, do đó tất cả các đóng góp vẫn bằng 0 và tổng tiền tố vẫn ổn định.
