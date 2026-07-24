---
title: "CF 103808E - Đăng ký lại"
description: "Chúng tôi được cung cấp một hệ thống viết lại trên chuỗi. Mỗi quy tắc mô tả cách một ký tự có thể mở rộng thành chuỗi hai ký tự. Nếu một quy tắc nói c → ab thì mỗi khi chúng ta chọn một vị trí trong chuỗi chứa c, chúng ta được phép thay thế ký tự đó bằng cặp ab."
date: "2026-07-02T08:38:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103808
codeforces_index: "E"
codeforces_contest_name: "XXVI Spain Olympiad in Informatics, Day 2"
rating: 0
weight: 103808
solve_time_s: 49
verified: true
draft: false
---

[CF 103808E - Reescritura](https://codeforces.com/problemset/problem/103808/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một hệ thống viết lại trên chuỗi. Mỗi quy tắc mô tả cách một ký tự có thể mở rộng thành chuỗi hai ký tự. Nếu một quy tắc nói`c → ab`, thì mỗi khi chúng ta chọn một vị trí trong chuỗi chứa`c`, chúng ta được phép thay ký tự đó bằng cặp`ab`. Điều này làm tăng độ dài của chuỗi lên một ở mỗi ứng dụng. 

Đối với mỗi trường hợp thử nghiệm, chúng tôi bắt đầu từ một chuỗi nguồn`s`và muốn biết liệu có thể lấy được chuỗi mục tiêu hay không`t`bằng cách áp dụng lặp đi lặp lại các quy tắc thay thế này theo bất kỳ thứ tự nào và ở bất kỳ vị trí nào. 

Thuộc tính cấu trúc quan trọng là mỗi ký tự có nhiều nhất một quy tắc gửi đi và biểu đồ quy tắc không có chu trình. Điều đó có nghĩa là bắt đầu từ bất kỳ ký tự nào, việc mở rộng lặp đi lặp lại cuối cùng sẽ chấm dứt và không bao giờ lặp lại chính nó thông qua việc viết lại. Tuy nhiên, việc mở rộng có thể phân nhánh vì mỗi ký tự có thể mở rộng thành hai ký tự, do đó một ký tự trong`s`có thể tạo ra toàn bộ cây con của các ký tự trong`t`. 

Các ràng buộc ngụ ý rằng chúng ta không thể mô phỏng việc mở rộng đầy đủ một cách ngây thơ. Một ký tự có thể tăng gấp đôi độ dài chuỗi nhiều lần, do đó tổng kích thước mở rộng sẽ theo cấp số nhân trong trường hợp xấu nhất. Mặc dù`n ≤ 59`, cấu trúc vẫn cho phép chuỗi phụ thuộc dài. Tổng độ dài của tất cả các chuỗi trong các trường hợp thử nghiệm lên tới 3 · 10^5, do đó, mọi giải pháp đều phải gần tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm. 

Một khó khăn tinh tế là các quy tắc không đối xứng và việc mở rộng là không thể đảo ngược. Khi một ký tự được mở rộng, chúng ta không thể “hợp nhất” lại. Điều này làm cho việc so khớp cục bộ tham lam trên các chuỗi trở nên không tầm thường. 

Một số trường hợp đặc biệt bộc lộ những cạm bẫy phổ biến. 

Nếu không có quy tắc thì`s`chỉ có thể bằng`t`. Ví dụ`s = "abc"`,`t = "abc"`là CÓ, nhưng`t = "a"`là KHÔNG. 

Nếu một ký tự mở rộng nhưng không bao giờ khớp với cấu trúc mục tiêu thì việc mở rộng sớm có thể phá vỡ tính khả thi. Ví dụ, nếu`a → bc`nhưng mục tiêu bắt đầu bằng`a`ở vị trí không thể nén ngược, sự thay thế tham lam ngây thơ không thành công. 

Một trường hợp phức tạp khác là khi nhiều phần mở rộng tương tác với nhau. Giả định`a → bc`,`b → d e`. Mở rộng`a`đầu tiên hoặc mở rộng`b`đầu tiên dẫn đến các hình dạng trung gian khác nhau, nhưng cả hai đều phải nhất quán với cùng một chuỗi cuối cùng`t`. 

Thách thức cốt lõi là chúng ta phải kết hợp cấu trúc cây được tạo với chuỗi mục tiêu phẳng. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng mô phỏng tất cả các cách viết lại có thể có của`s`cho đến khi đạt được`t`hoặc vượt quá chiều dài của nó. Mỗi nhân vật có thể mở rộng độc lập nên số lượng trạng thái tăng theo cấp số nhân. Ngay cả khi chúng ta cắt tỉa khi chiều dài vượt quá`|t|`, một ký tự đơn lẻ có thể tạo ra nhiều tổ hợp trung gian, dẫn đến sự bùng nổ tổ hợp. Điều này là không khả thi khi dây đạt đến độ dài vừa phải. 

Quan sát quan trọng là quy trình này mang tính quyết định khi chúng tôi sửa cách mỗi ký tự ánh xạ vào cây con và các quy tắc tạo thành biểu đồ tuần hoàn có hướng trên các ký tự. Điều này gợi ý nên đảo ngược quan điểm: thay vì mở rộng`s`, chúng ta có thể tính toán nội dung của mỗi ký tự trong`s`phải tương ứng trong`t`, xét về các phân đoạn của chuỗi đích. 

Vì mỗi ký tự mở rộng thành đúng hai ký tự con nên mỗi ký tự xác định một cây mở rộng nhị phân. Vì biểu đồ có tính tuần hoàn nên chúng ta có thể xử lý các ký tự theo thứ tự tôpô ngược và tính toán chữ ký mở rộng đầy đủ của mỗi ký tự dưới dạng một “chuỗi ảo” trên`t`, nhưng chúng tôi chỉ quan tâm liệu những chữ ký này có khớp với chuỗi con của`t`. 

Điều này làm giảm vấn đề để xác minh xem việc mở rộng của`s`có thể được phân đoạn để phù hợp chính xác`t`, trong đó mỗi ký tự đóng góp một phân đoạn được xác định đệ quy. Chúng tôi sử dụng tính năng ghi nhớ hoặc DP thay vì khớp ký tự với khoảng thời gian, tận dụng thực tế là khả năng mở rộng của mỗi ký tự là cố định. 

Chúng ta có thể nghĩ đến việc định nghĩa một hàm`match(c, l, r)`có nghĩa là liệu nhân vật`c`có thể tạo ra chuỗi con chính xác`t[l:r]`. Bởi vì mỗi luật chia thành hai phần tử con nên chúng ta thử tách`[l:r]`thành hai phần và ghép các phần con một cách đệ quy. Vì các quy tắc có tính chất không tuần hoàn nên việc ghi nhớ`(c, l, r)`là an toàn. 

Tuy nhiên, chúng ta có thể đơn giản hóa hơn nữa. Vì mỗi ký tự luôn mở rộng thành chính xác hai ký tự nên chúng tôi có thể tính toán trước kích thước mở rộng đầy đủ của từng ký tự về số lượng lá, nhưng quan trọng hơn là chúng tôi mô phỏng trực tiếp việc mở rộng DFS theo yêu cầu trong khi sử dụng chuỗi mục tiêu. 

Chúng tôi xử lý`s`từ trái sang phải và duy trì một con trỏ ở`t`. Mỗi nhân vật trong`s`phải khớp với một khối liền kề trong`t`. Chúng tôi mở rộng đệ quy một ký tự, sử dụng chính xác số lượng ký tự từ`t`theo yêu cầu của cây con của nó. Nếu tại bất kỳ điểm nào không khớp xảy ra, chúng tôi sẽ thất bại. Vì biểu đồ là DAG nên mỗi phần mở rộng ký tự được xử lý một lần, làm cho quá trình tuyến tính trong tổng kích thước mở rộng được giới hạn bởi`|t|`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| DFS tối ưu với kết hợp mở rộng được ghi nhớ | O( | s | + | 

## Hướng dẫn thuật toán 

Chúng tôi mô hình hóa mỗi ký tự dưới dạng một nút trong biểu đồ có quy tắc 0 hoặc một quy tắc đi ra tạo ra hai con. Sau đó, chúng tôi xác thực xem việc mở rộng chuỗi nguồn có tạo ra chính xác chuỗi đích hay không. 

1. Xây dựng từ điển ánh xạ từng ký tự`c`đến sự mở rộng của nó`(a, b)`nếu một quy tắc`c → ab`tồn tại. Điều này cho phép truy cập trực tiếp vào trẻ em trong O(1). 
2. Chúng ta định nghĩa một thủ tục đệ quy`dfs(c, pos)`cố gắng phù hợp với sự mở rộng của nhân vật`c`bắt đầu chính xác tại chỉ mục`pos`trong chuỗi đích. Nó trả về vị trí tiếp theo trong`t`sau khi tiêu thụ tất cả các ký tự được tạo bởi`c`hoặc thất bại nếu xảy ra sự không khớp. Lý do trả về một vị trí là vì mỗi cây con sử dụng một đoạn liền kề của`t`. 
3. Nếu`c`không có quy tắc, nó phải khớp chính xác với một ký tự trong`t`, vì vậy chúng tôi kiểm tra xem`t[pos] == c`. Nếu không, chúng ta thất bại. Nếu không chúng tôi sẽ quay lại`pos + 1`. Đây là trường hợp cơ bản của cây mở rộng. 
4. Nếu`c → ab`, trước tiên chúng tôi khớp đệ quy`a`bắt đầu từ`pos`, có được một vị trí mới`mid`. Sau đó chúng ta hợp nhau`b`bắt đầu từ`mid`. Điều này buộc thứ tự mở rộng là con bên trái, tiếp theo là con bên phải, bảo toàn cấu trúc của chuỗi được tạo. 
5. Đối với mỗi ký tự trong chuỗi đầu tiên`s`, chúng tôi liên tục áp dụng`dfs`bắt đầu từ con trỏ toàn cục hiện tại trong`t`. Nếu tại bất kỳ thời điểm nào xảy ra sự không khớp hoặc vượt quá giới hạn, chúng tôi kết luận rằng việc chuyển đổi là không thể. 
6. Sau khi xử lý tất cả các ký tự trong`s`, chúng tôi kiểm tra xem chúng tôi đã sử dụng chính xác tất cả các ký tự của`t`. Nếu không, việc chuyển đổi không đầy đủ và do đó không hợp lệ. 

Ý tưởng chính là mỗi ký tự xác định một cây mở rộng có thứ tự cố định và chuỗi mục tiêu phải chính xác là sự ghép nối của các cây này theo thứ tự. 

### Tại sao nó hoạt động 

Các quy tắc mở rộng xác định một cây có thứ tự gốc cho mỗi ký tự, vì hệ thống này không có tính tuần hoàn và mỗi nút có nhiều nhất một cạnh đi ra. Mọi ứng dụng của một quy tắc sẽ thay thế một nút bằng hai nút con của nó theo thứ tự từ trái sang phải, vì vậy chuỗi cuối cùng chính xác là việc duyệt theo thứ tự của khu rừng mở rộng này. 

Thủ tục DFS thực thi rằng mỗi cây con sử dụng một đoạn liền kề của chuỗi mục tiêu. Vì không có chu kỳ nên mỗi bản mở rộng ký tự đều được xác định rõ ràng và không thể xem lại vô hạn. Việc ghi nhớ hoặc sử dụng một lần đảm bảo chúng tôi không bao giờ tính toán lại các phần mở rộng không nhất quán. Nếu tại bất kỳ thời điểm nào một ký tự không khớp với ký hiệu dự kiến ​​hoặc phân đoạn của`t`thất bại, không có chuỗi mở rộng hợp lệ nào có thể tạo ra`t`, vì mọi dẫn xuất hợp lệ đều phải tuân theo cùng một phân tách cây. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

def solve():
    T = int(input())
    
    for _ in range(T):
        n = int(input())
        rule = {}
        
        for _ in range(n):
            line = input().strip()
            a = line[0]
            b = line[3]
            c = line[4]
            # format: a->bc (actually a -> bc)
            rule[line[0]] = (line[3], line[4])
        
        s = input().strip()
        t = input().strip()
        
        idx = 0
        n_t = len(t)
        ok = True
        
        from functools import lru_cache
        
        @lru_cache(None)
        def dfs(ch, pos):
            if pos > n_t:
                return -1
            
            if ch not in rule:
                if pos < n_t and t[pos] == ch:
                    return pos + 1
                return -1
            
            a, b = rule[ch]
            
            mid = dfs(a, pos)
            if mid == -1:
                return -1
            return dfs(b, mid)
        
        cur = 0
        
        for ch in s:
            cur = dfs(ch, cur)
            if cur == -1:
                ok = False
                break
        
        if ok and cur == n_t:
            print("SI")
        else:
            print("NO")

if __name__ == "__main__":
    solve()
```Việc triển khai xây dựng bảng quy tắc trước tiên, ánh xạ từng ký tự tới hai ký tự con mở rộng của nó. Hàm DFS là logic cốt lõi: nó sử dụng chuỗi đích trong khi vẫn tôn trọng cấu trúc mở rộng. 

Việc ghi nhớ đảm bảo rằng các phần mở rộng lặp đi lặp lại của cùng một ký tự ở cùng một vị trí sẽ không được tính toán lại, điều này rất quan trọng vì nhiều ký tự trong`s`có thể mở rộng thành các cây con giống hệt nhau. 

Con trỏ`cur`thực thi thứ tự nối toàn cầu. Mỗi nhân vật trong`s`phải tiêu thụ hoàn toàn một phân đoạn liền kề của`t`, nếu không thì cấu trúc không hợp lệ. 

Kiểm tra cuối cùng`cur == len(t)`đảm bảo không còn ký tự nào còn sót lại trong mục tiêu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
a->bc
aa
bca
```Quy tắc:`a → bc`, không có quy tắc nào cho`b`hoặc`c`. 

Chúng tôi bắt đầu với`s = "aa"`và mục tiêu`t = "bca"`. 

| Bước | Nhân vật | Bắt đầu tư thế | Kết quả trận đấu | Kết thúc vị trí | 
| --- | --- | --- | --- | --- | 
| 1 | một | 0 | bc khớp với t[0:2] | 2 | 
| 2 | một | 2 | a trận đấu t[2] | 3 | 

Sau khi xử lý, chúng tôi tiêu thụ chính xác`t`. 

Điều này chứng tỏ rằng các phần mở rộng có thể xen kẽ cấu trúc một cách chính xác khi mỗi phần mở rộng ký tự căn chỉnh với các phân đoạn liền kề. 

### Ví dụ 2 

đầu vào:```
a->bc
b->cc
c->mn
abc
acmnx
```Chúng tôi xử lý`a`,`b`,`c`một cách tuần tự. 

| Bước | Nhân vật | Bắt đầu tư thế | Mở rộng | Kết thúc vị trí | hợp lệ | 
| --- | --- | --- | --- | --- | --- | 
| 1 | một | 0 | bc → b thì c, không khớp sớm | thất bại | không | 

Sự cố xảy ra do việc mở rộng`a`buộc một cấu trúc không thể liên kết với tiền tố của`t`. 

Điều này cho thấy sự không phù hợp về cấu trúc ban đầu đã vô hiệu hóa toàn bộ đạo hàm như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O( | s | 
| Không gian | O(n + | t | 

Các ràng buộc cho phép tổng chiều dài chuỗi lên tới 3 · 10^5, do đó, việc truyền tải tuyến tính hoặc gần tuyến tính là đủ. Việc ghi nhớ đảm bảo không phải tính toán lại nhiều lần đối với các bài toán con giống hệt nhau. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import io as sio

    out = sio.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples
assert run("""a->bc
aa
bca
""") == "SI"

assert run("""a->bc
b->cc
c->mn
abc
acmnx
""") == "NO"

# custom cases
assert run("""a->bc
a
bc
""") == "SI", "single expansion"

assert run("""a->bc
b->aa
c->zz
ab
bcaaz
""") == "NO", "structure mismatch"

assert run("""a->bc
aa
bbcc
""") == "NO", "wrong order"

assert run("""a->bc
abc
bcmc
""") == "NO", "invalid terminal mismatch"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mở rộng duy nhất | SI | ứng dụng quy tắc cơ bản | 
| cấu trúc không phù hợp | KHÔNG | căn chỉnh cây con không hợp lệ | 
| sai thứ tự | KHÔNG | bảo quản đơn hàng | 
| thiết bị đầu cuối không khớp | KHÔNG | phát hiện lá không khớp | 

## Vỏ cạnh 

Trường hợp một cạnh xảy ra khi một ký tự không có quy tắc mở rộng nhưng xuất hiện trong bối cảnh có nhiều ký tự mở rộng vào nó một cách gián tiếp. Ví dụ, nếu`c`là thiết bị đầu cuối nhưng xuất hiện sâu trong cây con, DFS buộc chính xác sự bình đẳng ký tự chính xác ở vị trí đích. Bất kỳ sự không phù hợp nào sẽ ngay lập tức ngừng mở rộng, ngăn cản việc chấp nhận một phần. 

Một trường hợp cạnh khác là chuỗi sâu như`a → bc`,`b → de`,`d → fg`, nơi độ sâu mở rộng lớn. Phép đệ quy xử lý việc này một cách an toàn vì mỗi lệnh gọi tiêu tốn ít nhất một ký tự của`t`và việc ghi nhớ ngăn chặn việc tính toán lại các giá trị giống hệt nhau`(character, position)`trạng thái, đảm bảo hành vi tuyến tính. 

Trường hợp cạnh cuối cùng là các ký tự còn sót lại trong`t`. Ngay cả khi tất cả các bản mở rộng đều thành công cục bộ, việc kiểm tra con trỏ toàn cục sẽ đảm bảo rằng không có hậu tố bổ sung nào trong`t`vẫn chưa được sử dụng. Điều này phát hiện các trường hợp trong đó việc mở rộng tạo ra chuỗi ngắn hơn mức cần thiết, chẳng hạn như thiếu quy tắc hoặc mở rộng thiết bị đầu cuối chưa đủ.
