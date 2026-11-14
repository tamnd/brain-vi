---
title: "CF 1778C - Dây đàn hồi"
description: "Chúng ta được cung cấp hai chuỗi có độ dài bằng nhau và chúng ta được phép sửa đổi chuỗi đầu tiên theo một cách rất cụ thể. Mỗi lần chúng ta thay đổi một ký tự trong chuỗi đầu tiên, ký tự gốc ở vị trí đó sẽ được “ghi” vào một bộ đặc biệt."
date: "2026-06-09T11:34:12+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "brute-force", "strings"]
categories: ["algorithms"]
codeforces_contest: 1778
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 848 (Div. 2)"
rating: 1600
weight: 1778
solve_time_s: 83
verified: true
draft: false
---

[CF 1778C - Chuỗi linh hoạt](https://codeforces.com/problemset/problem/1778/C) 

**Đánh giá:** 1600 
**Tags:** bitmasks, vũ phu, chuỗi 
**Thời gian giải:** 1 phút 23s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp hai chuỗi có độ dài bằng nhau và chúng ta được phép sửa đổi chuỗi đầu tiên theo một cách rất cụ thể. Mỗi lần chúng ta thay đổi một ký tự trong chuỗi đầu tiên, ký tự gốc ở vị trí đó sẽ được “ghi” vào một bộ đặc biệt. Bộ này theo dõi những ký tự gốc mà chúng tôi đã từng ghi đè và kích thước của nó bị hạn chế bởi một giới hạn$k$. 

Khi kết thúc tất cả các sửa đổi, chúng tôi so sánh chuỗi đầu tiên đã sửa đổi với chuỗi thứ hai. Chúng tôi đếm có bao nhiêu chuỗi con giống hệt nhau giữa chúng và chúng tôi muốn tối đa hóa số lượng đó đồng thời đảm bảo rằng số lượng ký tự riêng biệt từng được thêm vào tập hợp đã ghi không vượt quá$k$. 

Một cách hữu ích để diễn giải lại thao tác là mọi vị trí đều giữ nguyên ký tự ban đầu hoặc được “tha thứ” bằng cách trả chi phí thêm ký tự đó vào bộ. Khi một ký tự đã có trong bộ, tất cả các lần xuất hiện của ký tự đó có thể được sửa đổi tự do mà không phải trả thêm phí. 

Quan sát cấu trúc quan trọng là ràng buộc được đặt là toàn cục đối với các ký tự chứ không phải vị trí. Vì có tối đa 10 ký tự riêng biệt trong chuỗi ban đầu nên mọi quyết định thực sự là về việc chọn tập hợp con nào trong số các ký tự này mà chúng ta sẵn sàng “chi” ngân sách cho. 

Các ràng buộc ngụ ý sự bùng nổ tổ hợp trên các tập hợp con vẫn khả thi. Với tối đa 10 ký tự, có nhiều nhất$2^{10} = 1024$tập hợp con và$n$trên tất cả các trường hợp thử nghiệm là$10^5$, vì vậy một$O(n \cdot 2^{10})$giải pháp có thể chấp nhận được. Bất cứ điều gì bậc hai trong$n$sẽ thất bại ngay lập tức. 

Trường hợp cạnh tinh tế xuất hiện khi$k = 0$. Trong trường hợp này, chúng ta không bao giờ có thể sửa đổi một ký tự có giá trị ban đầu khác với căn chỉnh đích, nghĩa là câu trả lời chỉ phụ thuộc vào các vị trí trong đó$a[i] = b[i]$. Một cách tiếp cận ngây thơ cho rằng chúng ta có thể tự do ghi đè những phần không khớp sẽ tạo ra kết quả không chính xác ở đây. 

Một trường hợp góc khác xảy ra khi$k$đủ lớn để bao gồm tất cả các ký tự riêng biệt trong$a$. Sau đó, chúng ta có thể thay đổi bất kỳ vị trí nào một cách hiệu quả mà không bị hạn chế, khiến kết quả tối ưu chỉ phụ thuộc vào sự liên kết cấu trúc với$b$và bất kỳ logic tham lam từng phần nào giả định các quyết định cục bộ đều có thể bỏ lỡ mức tối ưu toàn cục. 

## Phương pháp tiếp cận 

Một giải pháp vũ phu sẽ thử mọi cách để chọn vị trí nào cần sửa đổi và ký tự nào nằm trong bộ bị cấm$Q$. Đối với mỗi cấu hình, chúng tôi sẽ mô phỏng chuỗi cuối cùng và đếm các chuỗi con phù hợp với$b$. Điều này ngay lập tức trở nên không khả thi bởi vì ngay cả đối với cố định$k$, số lượng tập hợp con của các vị trí là theo cấp số nhân trong$n$và việc đếm chuỗi con chính nó là$O(n^2)$. Sự phức tạp tổng thể sẽ sụp đổ ngay cả khi có những đầu vào nhỏ. 

Sự đơn giản hóa chính là chuyển trọng tâm từ vị trí sang ký tự. Vì chi phí phát sinh cho mỗi ký tự gốc riêng biệt được sử dụng trong sửa đổi, không phải cho mỗi thao tác, nên vấn đề trở thành việc chọn một tập hợp con các ký tự từ tối đa 10 ký tự có sẵn trong$a$. Khi một tập hợp con được cố định, chúng ta có thể coi tất cả các vị trí chứa các ký tự đó là có thể sửa đổi tự do. 

Đối với bất kỳ tập hợp con được chọn nào, chúng ta có thể tính toán số lượng chuỗi con phù hợp tốt nhất có thể đạt được một cách hiệu quả bằng cách giảm vấn đề thành phân loại nhị phân theo các vị trí: tại mỗi chỉ mục, hoặc chúng ta buộc phải bình đẳng với$b[i]$hoặc chúng ta coi nó là linh hoạt và cố gắng tối đa hóa sự đóng góp. 

Cái nhìn sâu sắc quan trọng là việc đếm đẳng thức chuỗi con có thể được chuyển thành việc đếm các đóng góp theo cặp$(l, r)$, tương đương với việc tính tổng các lần chạy trong đó các ràng buộc tiền tố được thỏa mãn. Điều này dẫn đến việc giảm tiêu chuẩn: thay vì đếm trực tiếp các chuỗi con, chúng tôi tính toán xem có bao nhiêu vị trí là “tốt” về việc liệu sự bình đẳng tiền tố có được duy trì hay không. 

Vì bộ ký tự nhỏ nên chúng ta lặp qua tất cả các tập con ký tự trong$a$, mô phỏng sự căn chỉnh tốt nhất thu được và lấy mức tối đa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | hàm mũ trong$n$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n \cdot 2^{10})$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên chúng tôi nén tập hợp các ký tự riêng biệt trong$a$, vì chỉ những điều đó mới quan trọng trong các quyết định. Hãy đếm số lượng của họ$m \le 10$. 

1. Gán từng ký tự riêng biệt trong$a$chỉ số từ 0 đến$m-1$. Điều này cho phép chúng tôi biểu diễn bất kỳ tập hợp con nào dưới dạng bitmask. Lý do điều này hoạt động là vì tất cả các thao tác chỉ phụ thuộc vào việc một nhân vật có được “kích hoạt” để sửa đổi hay không chứ không phụ thuộc vào hành vi cụ thể của vị trí. 
2. Tính toán trước một mảng để đánh dấu, đối với mỗi vị trí, xem$a[i]$bằng$b[i]$. Điều này nắm bắt sự đóng góp cơ bản mà không có bất kỳ sửa đổi nào. 
3. Lặp lại tất cả các tập con ký tự trong$a$sử dụng mặt nạ bit. Đối với mỗi mặt nạ, hãy hiểu nó là tập hợp các ký tự chúng tôi chọn để đưa vào$Q$. Nếu số lượng ký tự được chọn vượt quá$k$, chúng tôi loại bỏ mặt nạ này ngay lập tức vì nó vi phạm ràng buộc. 
4. Đối với mặt nạ cố định, mô phỏng hiệu ứng trên từng vị trí: nếu$a[i]$nằm trong mặt nạ, chúng ta có thể thoải mái thay đổi cho phù hợp$b[i]$, do đó vị trí đó trở thành "có thể cố định được". Nếu không, vị trí sẽ bị khóa và chỉ đóng góp nếu đã bằng nhau. 
5. Chuyển đổi mảng kết quả thành mảng khuếch đại trong đó mỗi vị trí đóng góp 1 nếu nó có thể bằng$b[i]$, nếu không thì là 0. Vấn đề giảm xuống việc đếm xem có bao nhiêu chuỗi con được mảng này hỗ trợ đầy đủ. 
6. Tính số chuỗi con hợp lệ bằng cách quét từ trái sang phải và duy trì độ dài của phân đoạn hợp lệ hiện tại. Mỗi lần chúng tôi mở rộng một đoạn độ dài hợp lệ$L$, chúng tôi thêm$L$cho câu trả lời vì tất cả các chuỗi con hậu tố kết thúc tại thời điểm đó đều hợp lệ. 
7. Theo dõi mức tối đa trên tất cả các mặt nạ. 

Ý tưởng cốt lõi là đối với một lựa chọn cố định các ký tự có thể sửa đổi, tính hợp lệ sẽ trở thành một vấn đề cấu trúc liền kề đơn giản trên các vị trí và việc đếm chuỗi con sẽ chuyển thành tích lũy tuyến tính. 

### Tại sao nó hoạt động 

Đối với bất kỳ tập hợp con ký tự cố định nào, mỗi vị trí độc lập sẽ có thể sửa được hoàn toàn hoặc bị ràng buộc vĩnh viễn. Tính độc lập này đảm bảo rằng tính hợp lệ của chuỗi con chỉ phụ thuộc vào việc tất cả các vị trí trong chuỗi con có thể sửa được hay đã khớp hay chưa. Quét tuyến tính đếm chính xác tất cả các chuỗi con hợp lệ vì mỗi vị trí kết thúc hợp lệ đều đóng góp chính xác số lượng vị trí bắt đầu hợp lệ trong phân đoạn liền kề hiện tại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        a = input().strip()
        b = input().strip()

        # compress characters of a (at most 10 distinct)
        chars = list(set(a))
        m = len(chars)
        idx = {c: i for i, c in enumerate(chars)}

        # precompute equality
        eq = [1 if a[i] == b[i] else 0 for i in range(n)]

        ans = 0

        # iterate all subsets of characters in a
        for mask in range(1 << m):
            if mask.bit_count() > k:
                continue

            # build transformed validity array
            ok = [0] * n
            for i in range(n):
                if eq[i]:
                    ok[i] = 1
                else:
                    if mask & (1 << idx[a[i]]):
                        ok[i] = 1

            # count valid substrings in ok array
            cur = 0
            total = 0
            for i in range(n):
                if ok[i]:
                    cur += 1
                    total += cur
                else:
                    cur = 0

            ans = max(ans, total)

        print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên nén bảng chữ cái của$a$vì toàn bộ không gian quyết định chỉ phụ thuộc vào các ký tự đó. Vòng lặp bitmask liệt kê tất cả các lựa chọn ký tự hợp lệ để đưa vào bộ sửa đổi, cắt bớt bất kỳ mặt nạ nào vượt quá$k$. 

Đối với mỗi mặt nạ, chúng tôi xây dựng một mảng nhị phân biểu thị những vị trí nào có thể được đặt bằng$b$. Một vị trí luôn tốt nếu nó đã phù hợp; nếu không thì nó chỉ trở nên tốt nếu nhân vật của nó nằm trong chiếc mặt nạ đã chọn. Điều này trực tiếp mã hóa ảnh hưởng của các ràng buộc hoạt động. 

Bước đếm chuỗi con sử dụng kỹ thuật tiêu chuẩn để đếm sự đóng góp của tất cả mảng con trong một mảng nhị phân: một chuỗi độ dài liền kề$L$đóng góp$L(L+1)/2$chuỗi con, được tích lũy tăng dần dưới dạng tổng hiện hành. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp đơn giản trong đó$a = \texttt{abc}$,$b = \texttt{abd}$, Và$k = 1$. 

Chúng tôi liệt kê mặt nạ trên các ký tự$\{a,b,c\}$. 

| mặt nạ | ký tự được phép | mảng hợp lệ | tổng số chuỗi con | 
| --- | --- | --- | --- | 
| 000 | {} | 110 | 3 | 
| 001 | {c} | 111 | 6 | 
| 010 | {b} | 110 | 3 | 
| 100 | {a} | 110 | 3 | 

Sự lựa chọn tốt nhất là che giấu nhân vật$c$, cho phép hiệu chỉnh hoàn toàn ở vị trí 3, mang lại tất cả các chuỗi con hợp lệ. 

Bây giờ hãy xem xét một trường hợp không có tính linh hoạt,$k=0$,$a=\texttt{abc}$,$b=\texttt{abd}$. 

| mặt nạ | ký tự được phép | mảng hợp lệ | tổng số chuỗi con | 
| --- | --- | --- | --- | 
| 000 | {} | 110 | 3 | 

Không thể sửa đổi, vì vậy chỉ những vị trí đã phù hợp mới đóng góp và câu trả lời đã được sửa. 

Những ví dụ này cho thấy việc lựa chọn mặt nạ tối ưu trực tiếp xác định những điểm không khớp nào có thể được sửa chữa và việc đếm chuỗi con là hệ quả tất định của lựa chọn đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \cdot 2^{m})$| mỗi tập hợp con tính toán lại quá trình quét tuyến tính$n$, với$m \le 10$| 
| Không gian |$O(n + m)$| mảng cho đầu vào, đẳng thức và ánh xạ ký tự | 

Từ$m \le 10$, số tập con nhiều nhất là 1024, làm cho giải pháp nhanh chóng một cách thoải mái đối với$n \le 10^5$trên tất cả các trường hợp thử nghiệm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n, k = map(int, input().split())
        a = input().strip()
        b = input().strip()

        chars = list(set(a))
        idx = {c:i for i,c in enumerate(chars)}
        eq = [1 if a[i]==b[i] else 0 for i in range(n)]

        best = 0
        for mask in range(1<<len(chars)):
            if mask.bit_count() > k:
                continue
            ok = [0]*n
            for i in range(n):
                if eq[i] or (mask & (1<<idx[a[i]])):
                    ok[i]=1
            cur = ans = 0
            for v in ok:
                if v:
                    cur += 1
                    ans += cur
                else:
                    cur = 0
            best = max(best, ans)

        out.append(str(best))
    return "\n".join(out)

# sample checks (abbreviated style)
assert run("1\n3 1\nabc\nabd\n") == "6"
assert run("1\n3 0\nabc\nabd\n") == "3"
assert run("1\n3 10\nabc\nabd\n") == "6"
assert run("1\n4 1\nabcd\naxcb\n") == "6"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 0 abc abd | 3 | không được phép sửa đổi | 
| 3 1 abc abd | 6 | sửa chữa ký tự đơn tối ưu | 
| 4 1 abcd axcb | 6 | cấu trúc không phù hợp không cần thiết | 

## Vỏ cạnh 

Khi nào$k = 0$, vòng lặp mặt nạ chỉ cho phép tập hợp trống nên thuật toán giảm xuống việc quét mảng đẳng thức ban đầu. Trên đầu vào$a=\texttt{abc}$,$b=\texttt{abd}$, mảng hợp lệ là$110$, tạo ra chính xác 3 chuỗi con, phù hợp với hành vi mong đợi. 

Khi$k$lớn chẳng hạn$k \ge m$, mặt nạ đầy đủ sẽ trở nên hợp lệ và biến mọi vị trí thành một vị trí có thể sửa được. Vì$a=\texttt{xbb}$,$b=\texttt{xcd}$, việc chọn tất cả các ký tự sẽ đảm bảo tất cả các vị trí có thể được căn chỉnh, tạo ra số chuỗi con tối đa.
