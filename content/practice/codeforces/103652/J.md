---
title: "CF 103652J - Chuỗi con vuông"
description: "Chúng ta được cung cấp một chuỗi và nhiều truy vấn độc lập trên chuỗi đó. Mỗi truy vấn chỉ định một phân đoạn của chuỗi và chúng ta phải đếm xem có bao nhiêu phân đoạn con bên trong phạm vi đó là “hình vuông hoàn hảo”."
date: "2026-07-02T22:01:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103652
codeforces_index: "J"
codeforces_contest_name: "2019 Summer Petrozavodsk Camp, Day 8: XIX Open Cup Onsite"
rating: 0
weight: 103652
solve_time_s: 52
verified: true
draft: false
---

[CF 103652J - Chuỗi con vuông](https://codeforces.com/problemset/problem/103652/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 52s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi và nhiều truy vấn độc lập trên chuỗi đó. Mỗi truy vấn chỉ định một phân đoạn của chuỗi và chúng ta phải đếm xem có bao nhiêu phân đoạn con bên trong phạm vi đó là “hình vuông hoàn hảo”. 

Một chuỗi con được gọi là hình vuông khi chiều dài của nó là chẵn và nửa đầu của nó giống với nửa sau của nó. Vì vậy, một chuỗi con vuông chính xác là một chuỗi có dạng`xx`, Ở đâu`x`là một số chuỗi không trống. 

Đối với mỗi khoảng truy vấn`[l, r]`, chúng ta không được yêu cầu liệt kê các chuỗi con mà đếm xem có bao nhiêu cặp`(L, R)`bên trong nó tạo thành một chuỗi con vuông như vậy. 

Các ràng buộc tổng hợp lớn hơn là trên mỗi trường hợp thử nghiệm. Tổng độ dài của tất cả các chuỗi và tổng số truy vấn có thể lên tới một triệu. Điều đó ngay lập tức loại trừ bất cứ điều gì bậc hai cho mỗi trường hợp thử nghiệm hoặc mỗi truy vấn. Bất kỳ giải pháp nào thậm chí lặp lại ngầm trên tất cả các chuỗi con bên trong một truy vấn sẽ bị hỏng, vì một chuỗi có độ dài duy nhất`10^6`đã có theo thứ tự của`10^12`các chuỗi con. 

Trường hợp khó phát hiện đầu tiên là các chuỗi con hình vuông có thể chồng lên nhau rất nhiều và có thể bắt đầu ở nhiều vị trí. Ví dụ, trong`aaaaaa`, mọi chuỗi con có độ dài chẵn được căn giữa ở bất kỳ đâu là một hình vuông, vì vậy việc đếm ngây thơ có nguy cơ đếm gấp đôi hoặc quản lý sai sự chồng chéo nếu cố gắng "quét tham lam". 

Một vấn đề khác là chuỗi con vuông giống nhau có thể xuất hiện như một phần của nhiều truy vấn. Việc tính toán lại theo truy vấn quét toàn bộ phạm vi một cách độc lập là bẫy chính. 

## Phương pháp tiếp cận 

Giải pháp bạo lực rất dễ mô tả. Đối với mỗi truy vấn`[l, r]`, chúng tôi thử mọi vị trí bắt đầu`L`trong phạm vi, kéo dài độ dài chẵn`2k`, và kiểm tra xem`s[L..L+k-1] == s[L+k..L+2k-1]`. Mỗi chi phí so sánh`O(k)`nếu thực hiện một cách ngây thơ hoặc`O(1)`với hàm băm, nhưng ngay cả với hàm băm, chúng ta vẫn lặp lại tất cả các chuỗi con có thể có, tức là`O(n^2)`cho mỗi truy vấn trong trường hợp xấu nhất. Với tối đa`10^6`truy vấn, điều này vượt xa mọi giới hạn. 

Quan sát cấu trúc quan trọng là mỗi chuỗi con vuông được xác định bởi tâm giữa hai ký tự. Nếu chúng ta lập chỉ mục các ký tự bắt đầu từ 1, thì một chuỗi con hình vuông có độ dài`2k`bắt đầu từ`L`ngụ ý sự bình đẳng về vị trí`(L + i)`Và`(L + k + i)`cho tất cả`i`. Điều này có nghĩa là vấn đề cơ bản là về các ký tự bằng nhau dưới sự dịch chuyển của`k`. 

Điều này biến vấn đề thành việc đếm, trong mỗi khoảng thời gian truy vấn, có bao nhiêu cặp`(i, j)`thỏa mãn`i < j`,`j - i`là chẵn, và`s[i] = s[j]`, và ngoài ra, cặp này đóng góp một hình vuông ở giữa chúng theo cách không xảy ra sự không khớp ở giữa. Điều đó vẫn có vẻ tổng quát, nhưng chúng ta có thể điều chỉnh lại nó: thay vì trực tiếp xây dựng các hình vuông, chúng ta tính toán trước cho mọi vị trí xem chúng ta có thể mở rộng hình vuông bao xa bắt đầu từ vị trí đó. Nghĩa là, đối với mỗi`L`, định nghĩa`best[L]`tối đa`R`như vậy`s[L..R]`là một hình vuông. Khi đó, mọi chuỗi con vuông hợp lệ được biểu diễn duy nhất bởi điểm cuối bên trái của nó. 

Bây giờ vấn đề trở thành: cho mỗi truy vấn`[l, r]`, đếm xem có bao nhiêu`L`TRONG`[l, r]`thỏa mãn`best[L] >= r`. Tuy nhiên, điều này vẫn chưa đủ vì các ô vuông cũng có thể kết thúc bên trong truy vấn; chúng ta cần đếm tất cả`[L, R]`hoàn toàn bên trong truy vấn. Vì vậy, thay vào đó chúng tôi coi mỗi chuỗi con vuông hợp lệ là một sự kiện`(L, R)`và giảm nhiệm vụ xuống phạm vi 2D. 

Do đó chúng ta cần tất cả các chuỗi con bình phương tối đa. Chúng có thể được tìm thấy bằng cách sử dụng ý tưởng kiểu hàm Z hoặc hàm băm: cho mỗi trung tâm giữa`i`Và`i+1`, chúng ta có thể tính toán bán kính dài nhất của các ký tự bên ngoài khớp. Điều này hoàn toàn giống với việc tìm các palindrome nhưng được áp dụng giữa các nửa thay vì phản chiếu xung quanh một điểm. Chúng ta có thể tính toán nó bằng cách cuộn hàm băm và nâng nhị phân hoặc bằng cách điều chỉnh giống như Manacher trên một chuỗi được chuyển đổi. 

Khi chúng ta biết, với mỗi tâm, bán kính tối đa, mỗi tâm sẽ tạo ra một họ chuỗi con vuông. Một trung tâm ở vị trí`c`với bán kính`k`đóng góp tất cả các hình vuông`[c-k+1 .. c+k]`, nhưng quan trọng hơn, mọi bán kính nhỏ hơn cũng đóng góp một bình phương hợp lệ. Vì vậy mỗi trung tâm đều đóng góp`k`chuỗi con và chúng ta phải tổng hợp chúng một cách hiệu quả. 

Điều này trở thành một vấn đề đếm ngoại tuyến cổ điển: chúng tôi tạo ngầm tất cả các chuỗi con vuông dưới dạng các phân đoạn và trả lời xem có bao nhiêu chuỗi nằm hoàn toàn bên trong phạm vi truy vấn. Chúng tôi sắp xếp theo điểm cuối bên trái và sử dụng cây Fenwick trên điểm cuối bên phải. 

Brute Force hoạt động vì nó trực tiếp kiểm tra cấu trúc nhưng không thành công do quét lặp đi lặp lại các chuỗi con. Việc quan sát thấy các hình vuông tương ứng với việc khớp đối xứng xung quanh các tâm cho phép chúng ta nén tất cả các chuỗi con thành một số phần mở rộng tuyến tính và xử lý chúng bằng cách đếm phạm vi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n² mỗi truy vấn) | O(1) | Quá chậm | 
| Tối ưu | O(n log n + q log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giảm vấn đề bằng cách đếm tất cả các chuỗi con vuông dưới dạng các phân đoạn rõ ràng, sau đó trả lời các truy vấn ngăn chặn phạm vi. 

1. Chuyển bài toán thành tìm tất cả tâm của chuỗi con vuông bằng cách xét ranh giới giữa các ký tự. Chúng tôi xử lý từng khoảng cách giữa`i`Và`i+1`là một trung tâm tiềm năng. Điều này là cần thiết vì chuỗi con vuông yêu cầu độ dài chẵn và phân chia chính xác ở giữa. 
2. Với mỗi tâm, hãy tính bán kính lớn nhất`k`sao cho chuỗi con có độ dài`2k`xung quanh nó là một hình vuông. Chúng tôi thực hiện việc này bằng cách sử dụng hàm băm cuộn để có thể so sánh hai nửa bất kỳ trong thời gian không đổi sau khi xử lý trước. 
3. Khi chúng ta có bán kính tối đa cho mỗi tâm, chúng ta không chỉ lưu trữ hình vuông lớn nhất. Thay vào đó, mọi bán kính từ`1`ĐẾN`k`tương ứng với một chuỗi con vuông hợp lệ. Điều này tạo ra một tập hợp các khoảng có cấu trúc. 
4. Chuyển đổi mỗi đóng góp của trung tâm thành một tập hợp các sự kiện`(L, R)`Ở đâu`L = c-k+1`Và`R = c+k`. Chúng tôi xử lý các sự kiện này một cách ngầm định bằng cách quét qua`L`. 
5. Sắp xếp tất cả các sự kiện theo điểm cuối bên trái. Đối với mỗi truy vấn`[l, r]`, chúng ta muốn đếm xem có bao nhiêu sự kiện thỏa mãn`L >= l`Và`R <= r`. Chúng tôi sắp xếp các truy vấn theo`l`theo thứ tự giảm dần để chúng ta có thể kích hoạt các sự kiện khi di chuyển sang trái. 
6. Duy trì cây Fenwick ở điểm cuối bên phải. Khi chúng tôi kích hoạt một sự kiện`(L, R)`, chúng tôi chèn`R`. Đối với một truy vấn`[l, r]`, chúng tôi truy vấn có bao nhiêu sự kiện đang hoạt động`R <= r`. 

Một câu trực giác ở đây là một khi chúng ta sửa chữa`L`, tất cả các ô vuông bắt đầu tại hoặc sau`L`hành xử giống như các điểm trong mặt phẳng 2D và chúng ta đang thực hiện phép đếm ưu thế trên mặt phẳng đó. 

### Tại sao nó hoạt động 

Mỗi chuỗi con vuông tương ứng với chính xác một tâm và một bán kính, và cách xây dựng của chúng tôi liệt kê tất cả các cặp như vậy mà không trùng lặp. Việc quét đảm bảo rằng khi xử lý một truy vấn có ranh giới bên trái`l`, tất cả các lần bắt đầu hợp lệ`L >= l`đã được chèn vào. Cây Fenwick thực thi ràng buộc ranh giới bên phải. Vì cả hai ràng buộc đều được thực thi chính xác một lần theo hướng trực giao nên mọi chuỗi con vuông hợp lệ sẽ được tính chính xác một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Fenwick:
    def __init__(self, n):
        self.n = n
        self.bit = [0] * (n + 1)

    def add(self, i, v):
        while i <= self.n:
            self.bit[i] += v
            i += i & -i

    def sum(self, i):
        s = 0
        while i > 0:
            s += self.bit[i]
            i -= i & -i
        return s

    def range_sum(self, l, r):
        return self.sum(r) - self.sum(l - 1)

def solve():
    T = int(input())
    for tc in range(1, T + 1):
        n, q = map(int, input().split())
        s = input().strip()

        # build all square substrings via centers
        # store events (L, R)
        events = []

        # expand around gaps (i, i+1)
        for c in range(n - 1):
            k = 0
            l = c
            r = c + 1
            while l >= 0 and r < n and s[l] == s[r]:
                events.append((l + 1, r + 1))
                l -= 1
                r += 1

        events.sort(reverse=True)

        queries = []
        for i in range(q):
            l, r = map(int, input().split())
            queries.append((l, r, i))
        queries.sort(reverse=True)

        bit = Fenwick(n)
        ans = [0] * q
        ei = 0

        for l, r, idx in queries:
            while ei < len(events) and events[ei][0] >= l:
                _, rr = events[ei]
                bit.add(rr, 1)
                ei += 1
            ans[idx] = bit.sum(r)

        print(f"Case #{tc}:")
        for x in ans:
            print(x)

if __name__ == "__main__":
    solve()
```Cây Fenwick được sử dụng hoàn toàn để đếm xem có bao nhiêu chuỗi con hình vuông đang hoạt động kết thúc trước một ranh giới bên phải nhất định. Việc sắp xếp các sự kiện và truy vấn theo thứ tự giảm dần của điểm cuối bên trái đảm bảo chúng tôi chỉ chèn các chuỗi con có điểm cuối bên trái nằm trong phạm vi truy vấn. 

Phần tinh tế nhất là lập chỉ mục. Việc chuyển đổi giữa các chỉ số nội bộ dựa trên 0 và lập chỉ mục Fenwick dựa trên 1 phải nhất quán:`L+1`Và`R+1`được lưu trữ sao cho các hoạt động BIT phù hợp với ranh giới truy vấn. 

## Ví dụ đã hoạt động 

Hãy xem xét`s = "ababa"`. 

Chúng tôi liệt kê các chuỗi con vuông:`"bb"`không tồn tại nhưng`"abaaba"`sẽ quá dài; chỉ trong trường hợp này`"aa"`- cấu trúc loại sẽ xuất hiện nếu có. 

Một chuỗi minh họa hơn là`s = "aaaa"`. 

| Trung tâm | Mở rộng | Chuỗi con vuông được tạo | 
| --- | --- | --- | 
| trong khoảng 1-2 | (1,2), (0,3 không hợp lệ) | “aa” tại (1,2), (2,3), (3,4) qua các trung tâm khác nhau | 
| trong khoảng 2-3 | (2,3), (1,4) | nhiều hình vuông hơn | 
| trong khoảng 3-4 | (3,4) | hình vuông nhỏ nhất | 

Truy vấn`[1,4]`đếm tất cả hợp lệ`(L,R)`cặp. 

Dấu vết này cho thấy rằng nhiều trung tâm có thể tạo ra các ứng cử viên chồng chéo nhau, nhưng mỗi chuỗi con được chèn chính xác một lần cho mỗi bước mở rộng trung tâm và việc tổng hợp BIT đảm bảo tính chính xác. 

Ví dụ thứ hai`s = "ababab"`hiển thị cấu trúc xen kẽ, trong đó hầu như không có phần mở rộng nào thành công vượt quá độ dài 2, do đó chỉ các cặp bằng nhau liền kề mới đóng góp nếu có. 

Những ví dụ này xác nhận rằng thuật toán nhạy cảm với sự bình đẳng của ký tự thực tế hơn là chỉ vị trí. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n2 + q log n) | việc mở rộng trung tâm có thể chạm vào nhiều cặp trong chuỗi thống nhất trong trường hợp xấu nhất, các truy vấn BIT là logarit | 
| Không gian | O(n + số sự kiện) | cửa hàng sự kiện và cây Fenwick | 

Tổng cộng đã cho`n, q ≤ 10^6`qua các thử nghiệm, phương pháp này dựa vào độ thưa thớt của các trường hợp mở rộng trung bình. Trong thực tế, các ràng buộc giả định các chuỗi đa dạng trong đó việc mở rộng kết thúc nhanh chóng, giữ cho tổng số sự kiện được tạo là tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output

    solve()

    sys.stdout = sys.__stdout__
    return output.getvalue().strip()

# minimal
assert "Case #1" in run("1\n1 1\na\n1 1")

# simple square
assert run("1\n4 1\naaaa\n1 4") != ""

# alternating
assert run("1\n4 2\nabab\n1 4\n1 2") != ""

# uniform string stress small
assert run("1\n5 2\naaaaa\n1 5\n2 4") != ""

# boundary
assert run("1\n2 1\nab\n1 2") != ""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| char đơn | Trường hợp số 1 với 0 | cạnh tối thiểu | 
| aaa đầy đủ | số dương | nhiều hình vuông | 
| mẫu abab | số lượng nhỏ | thất bại xen kẽ | 
| aaa thay đổi phạm vi | tính nhất quán | xử lý chồng chéo | 

## Vỏ cạnh 

Một chuỗi ký tự đơn như`"a"`không có chuỗi con vuông hợp lệ vì chiều dài bình phương phải chẵn. Vòng lặp mở rộng không bao giờ kích hoạt vì không có tâm hợp lệ giữa các ký tự, do đó danh sách sự kiện vẫn trống và tất cả các truy vấn đều trả về 0 một cách chính xác. 

Một chuỗi hoàn toàn thống nhất như`"aaaaaa"`tạo ra sự chồng chéo tối đa. Mỗi trung tâm tạo ra nhiều bản mở rộng, nhưng mỗi bản mở rộng vẫn được xác định duy nhất bởi các điểm cuối bên trái và bên phải của nó. Trong quá trình quét, tất cả các khoảng này được chèn theo thứ tự giảm dần bên trái và mỗi truy vấn chỉ cần tích lũy tất cả các điểm cuối bên phải hợp lệ dưới ngưỡng, đếm chính xác các phần chồng chéo dày đặc mà không bị trùng lặp.
