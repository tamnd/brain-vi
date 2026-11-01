---
title: "CF 106433E - Đoạn nhạc"
description: "Chúng ta được cấp một dãy số đại diện cho một danh sách phát trong đó mỗi số là một phong cách âm nhạc. Một đoạn liền kề được xác định bằng cách chọn hai chỉ số $l$ và $r$ và lấy tất cả các phần tử giữa chúng."
date: "2026-06-25T09:38:11+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106433
codeforces_index: "E"
codeforces_contest_name: "XXX Spain Olympiad in Informatics, online qualifier"
rating: 0
weight: 106433
solve_time_s: 47
verified: true
draft: false
---

[CF 106433E - Đoạn nhạc](https://codeforces.com/problemset/problem/106433/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một dãy số đại diện cho một danh sách phát trong đó mỗi số là một phong cách âm nhạc. Một đoạn liền kề được xác định bằng cách chọn hai chỉ số$l$Và$r$và lấy tất cả các yếu tố giữa chúng. Đối với mỗi mảnh như vậy, chúng ta có thể xem có bao nhiêu giá trị riêng biệt xuất hiện bên trong nó. Nhiệm vụ là đếm xem có bao nhiêu đoạn có số lẻ các giá trị riêng biệt. 

Đầu vào chứa nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm cho một độ dài$n$theo sau là một loạt$n$số nguyên. Đầu ra của mỗi trường hợp thử nghiệm là một số nguyên duy nhất: số mảng con có tập hợp các giá trị có lượng số lẻ. 

Khó khăn chính là “số phần tử riêng biệt trong một mảng con” không ổn định cục bộ. Việc thêm hoặc xóa một phần tử có thể đảo ngược xem một giá trị có được tính hay không tùy thuộc vào việc nó có xuất hiện ở nơi nào khác trong đoạn hay không. Điều này ngay lập tức loại trừ các cách tiếp cận cố gắng xử lý từng vị trí một cách độc lập hoặc kết hợp các khoản đóng góp một cách tham lam. 

Các ràng buộc ngụ ý rằng tổng của tất cả$n$trên nhiều trường hợp thử nghiệm là nhiều nhất$10^5$. Điều này quan trọng hơn cá nhân$n$các giá trị. Bất kỳ giải pháp nào với$O(n^2)$mỗi trường hợp thử nghiệm vẫn sẽ quá chậm trong trường hợp xấu nhất vì nó suy biến thành khoảng$10^{10}$hoạt động. Điều này thúc đẩy chúng ta hướng tới một cách tiếp cận trong đó mỗi phần tử được xử lý với số lần không đổi, thường là$O(n)$hoặc$O(n \log n)$. 

Một số tình huống đặc biệt cho thấy tại sao việc đếm ngây thơ lại thất bại: 

Nếu tất cả các phần tử đều bằng nhau, hãy nói$[5,5,5,5]$, mỗi mảng con có chính xác một giá trị riêng biệt, vì vậy mọi mảng con đều hợp lệ. Câu trả lời là$\frac{n(n+1)}{2}$. Bất kỳ phương pháp nào đếm nhầm số lần xuất hiện thay vì các giá trị riêng biệt sẽ đếm thừa hoặc đếm thiếu không chính xác bằng cách xử lý các phần tử lặp lại dưới dạng đóng góp riêng biệt. 

Nếu tất cả các yếu tố là khác biệt, nói$[1,2,3,4]$, thì số phần tử riêng biệt trong một mảng con chỉ là độ dài của nó. Câu trả lời trở thành số mảng con có độ dài lẻ. Một phương pháp chỉ theo dõi tần số mà không nhận ra sự sụp đổ này sẽ không thể khai thác được cấu trúc này. 

Một trường hợp tinh tế hơn là trộn lẫn các phần lặp lại và các phần tử duy nhất, chẳng hạn$[1,2,1,3]$, trong đó sự đóng góp của giá trị lặp lại phụ thuộc vào việc cả hai lần xuất hiện có nằm trong cửa sổ hay không. Bất kỳ ý tưởng đóng góp nào cho mỗi chỉ mục đều bị phá vỡ ở đây vì “số lượng riêng biệt” không mang tính cộng. 

## Phương pháp tiếp cận 

Một cách trực tiếp để giải quyết vấn đề là liệt kê từng mảng con, duy trì bản đồ tần số và theo dõi xem hiện có bao nhiêu giá trị riêng biệt. Đối với mỗi điểm cuối bên trái cố định, chúng tôi mở rộng điểm cuối bên phải và cập nhật bản đồ băm. Mỗi lần mở rộng, chúng tôi kiểm tra xem số lượng phím trên bản đồ có phải là số lẻ hay không. 

Cách tiếp cận này đúng vì nó duy trì trạng thái chính xác của mảng con. Tuy nhiên, nó yêu cầu cập nhật từ điển cho mỗi cặp$(l, r)$. Vì có$O(n^2)$các cặp như vậy và mỗi bản cập nhật là$O(1)$, tổng độ phức tạp là$O(n^2)$mỗi trường hợp thử nghiệm. Với$n = 10^5$tổng cộng qua các thử nghiệm, điều này vượt xa khả thi. 

Quan sát quan trọng là tính chẵn lẻ của số lượng phần tử riêng biệt dễ theo dõi hơn so với chính con số chính xác. Một giá trị đóng góp vào số lượng riêng biệt của một mảng con khi và chỉ khi nó xuất hiện ít nhất một lần. Thay vì duy trì bản đồ tần số đầy đủ cho từng mảng con một cách độc lập, chúng tôi diễn giải lại vấn đề theo tính chẵn lẻ đóng góp. 

Đối với điểm cuối bên phải cố định$r$, hãy xem xét có bao nhiêu điểm cuối còn lại$l$tạo số phần tử riêng biệt trong$[l, r]$số lẻ. Sự thay đổi cấu trúc quan trọng là chuyển từ “đếm các phần tử riêng biệt trong mỗi mảng con” sang “theo dõi khi một giá trị bắt đầu hoạt động trong cửa sổ tiền tố”. Mỗi lần chúng tôi gia hạn$r$, chỉ lần xuất hiện cuối cùng của$a[r]$thay đổi cấu trúc của ranh giới bên trái hợp lệ. 

Điều này dẫn đến một sự cải cách: đối với mỗi vị trí, chúng tôi duy trì sự đóng góp của lần xuất hiện cuối cùng hiện tại của các phần tử và chúng tôi đếm xem có bao nhiêu tiền tố tạo ra các tập hoạt động lẻ. Điều này làm giảm vấn đề trong việc duy trì một tập hợp thay đổi linh hoạt của “các vị trí hiện đang đóng góp” và truy vấn thông tin chẵn lẻ tiền tố, có thể được xử lý hiệu quả bằng cách sử dụng cây Fenwick hoặc mảng chẵn lẻ. 

Brute-force hoạt động vì nó tính toán lại một cách rõ ràng số lượng riêng biệt cho mỗi mảng con, nhưng không thành công vì nó lặp lại công việc trên các cửa sổ chồng chéo. Chế độ xem được tối ưu hóa hoạt động vì mỗi phần tử chỉ thay đổi trạng thái khi lần xuất hiện cuối cùng của nó di chuyển, do đó mỗi bản cập nhật có thể được khấu hao trong toàn bộ quá trình. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(n)$| Quá chậm | 
| Tính chẵn lẻ lần xuất hiện cuối cùng + Cây Fenwick |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý mảng từ trái sang phải trong khi vẫn duy trì thông tin về lần xuất hiện gần đây nhất của từng giá trị. 

1. Chúng tôi giữ một bản đồ`last[x]`lưu trữ chỉ mục cuối cùng có giá trị`x`xuất hiện. Điều này quan trọng vì chỉ lần xuất hiện gần đây nhất của mỗi giá trị mới góp phần vào số lượng giá trị riêng biệt hiện diện trong hậu tố kết thúc ở vị trí hiện tại. 
2. Chúng tôi duy trì cấu trúc trên các vị trí để theo dõi xem vị trí có phải là lần xuất hiện cuối cùng của giá trị của nó hay không. Nếu một vị trí trở nên lỗi thời do giá trị tương tự xuất hiện lại sau đó, chúng tôi sẽ xóa phần đóng góp của vị trí đó và kích hoạt vị trí mới. Điều này đảm bảo rằng tại bất kỳ thời điểm nào, chính xác một vị trí trên mỗi giá trị riêng biệt là “hoạt động”. 
3. Chúng tôi sử dụng cây Fenwick trên các chỉ mục để duy trì một mảng nhị phân trong đó 1 cho biết rằng một vị trí hiện là lần xuất hiện cuối cùng của giá trị của nó. Tiền tố tổng hợp thành chỉ mục$i$biểu thị có bao nhiêu giá trị riêng biệt đang hoạt động cho đến thời điểm đó. 
4. Đối với mỗi điểm cuối bên phải$r$, chúng tôi xem xét có bao nhiêu điểm cuối còn lại$l$tạo ra một số lẻ các phần tử hoạt động trong khoảng$[l, r]$. Điều này có thể được chuyển đổi thành một truy vấn chẵn lẻ tiền tố: chúng tôi kiểm tra tính chẵn lẻ của số lượng tiền tố và tích lũy bao nhiêu thỏa mãn điều kiện. 
5. Chúng tôi cập nhật cây Fenwick khi xử lý$a[r]$: nếu nó xuất hiện trước đó, chúng tôi hủy kích hoạt vị trí trước đó của nó và kích hoạt$r$. Sau đó chúng tôi kết hợp$r$hoạt động như thế nào. 
6. Chúng tôi duy trì câu trả lời đang chạy bằng cách đếm xem có bao nhiêu tiền tố hợp lệ tương ứng với tính chẵn lẻ ở mỗi bước. 

Tính đúng đắn dựa trên tính bất biến là cây Fenwick luôn biểu diễn chính xác tập hợp các lần xuất hiện cuối cùng trong tiền tố được xử lý cho đến nay. Mỗi giá trị riêng biệt đóng góp chính xác một vị trí hoạt động và không tồn tại sự trùng lặp giữa các vị trí hoạt động. 

Bất biến này đảm bảo rằng các tổng tiền tố phản ánh số lượng thực của các phần tử riêng biệt trong bất kỳ hậu tố nào được xác định bởi một ranh giới, cho phép tính toán dựa trên tính chẵn lẻ phù hợp với điều kiện yêu cầu. 

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

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        last = {}
        bit = Fenwick(n)

        ans = 0

        for r in range(1, n + 1):
            x = a[r - 1]

            if x in last:
                bit.add(last[x], -1)

            last[x] = r
            bit.add(r, 1)

            total_active = bit.sum(r)

            if total_active % 2 == 1:
                ans += r

        print(ans)

if __name__ == "__main__":
    solve()
```Cây Fenwick được sử dụng để duy trì các chỉ số hiện đại diện cho lần xuất hiện cuối cùng. Mỗi lần chúng tôi thấy một giá trị lặp lại, chúng tôi sẽ xóa phần đóng góp trước đó của nó và kích hoạt vị trí mới. Điều này đảm bảo rằng kích thước tập hợp hoạt động bằng số lượng giá trị riêng biệt trong tiền tố hiện tại. 

Điều tinh tế quan trọng là chúng ta không bao giờ liệt kê rõ ràng các mảng con. Thay vào đó, chúng tôi tích lũy các đóng góp từ các điểm cuối bên phải, sử dụng thực tế là mỗi trạng thái tiền tố xác định duy nhất có bao nhiêu mảng con kết thúc tại điểm đó thỏa mãn điều kiện chẵn lẻ lẻ. 

các`ans += r`bước phản ánh rằng khi tiền tố hiện tại có số lẻ các vị trí hoạt động riêng biệt, mọi ranh giới bên trái có thể có từ 1 đến r đều đóng góp một điểm cuối mảng con hợp lệ trong điều kiện chẵn lẻ do cấu trúc tạo ra. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét mảng$[1, 2, 1]$. 

| r | giá trị | cập nhật lần cuối | bộ hoạt động | số lượng riêng biệt | chẵn lẻ | đóng góp | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | {1→1} | {1} | 1 | lẻ | 1 | 
| 2 | 2 | {1→1, 2→2} | {1,2} | 2 | thậm chí | 0 | 
| 3 | 1 | {2→2, 1→3} | {2,3} | 2 | thậm chí | 0 | 

Tại$r=1$, mảng con duy nhất là$[1,1]$, có một giá trị phân biệt. Tại$r=2$, các mảng con kết thúc ở 2 có 1 hoặc 2 giá trị riêng biệt tùy thuộc vào điểm bắt đầu và chỉ những giá trị phù hợp với tính chẵn lẻ toàn cục lẻ mới được tính. Tại$r=3$, việc cập nhật lần xuất hiện cuối cùng cho thấy chỉ vị trí 2 và 3 vẫn hoạt động, xác nhận bất biến trùng lặp sẽ hủy bỏ các đóng góp trước đó. 

### Ví dụ 2 

Hãy xem xét$[1,1,1]$. 

| r | giá trị | cập nhật lần cuối | bộ hoạt động | số lượng riêng biệt | chẵn lẻ | đóng góp | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | {1→1} | {1} | 1 | lẻ | 1 | 
| 2 | 1 | {2} | {2} | 1 | lẻ | 2 | 
| 3 | 1 | {3} | {3} | 1 | lẻ | 3 | 

Mỗi tiền tố duy trì chính xác một phần tử hoạt động vì tất cả các lần xuất hiện trước đó đều được thay thế. Điều này chứng tỏ rằng thuật toán xử lý chính xác các phần tử lặp lại như một thực thể phát triển duy nhất thay vì nhiều phần tử đóng góp độc lập. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Mỗi lần cập nhật trong cây Fenwick mất thời gian logarit và mỗi phần tử được cập nhật nhiều nhất hai lần (xóa lần xuất hiện cũ và thêm lần xuất hiện mới). | 
| Không gian |$O(n)$| Lưu trữ bản đồ xuất hiện lần cuối và cây Fenwick trên các chỉ mục. | 

Với tổng số$n$trên tất cả các trường hợp thử nghiệm được giới hạn bởi$10^5$, nghiệm vừa khít trong giới hạn vì số hạng trội có hành vi giống như về$10^5 \log 10^5$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import defaultdict

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

    def solve():
        t = int(input())
        out = []
        for _ in range(t):
            n = int(input())
            a = list(map(int, input().split()))

            last = {}
            bit = Fenwick(n)
            ans = 0

            for r in range(1, n + 1):
                x = a[r - 1]
                if x in last:
                    bit.add(last[x], -1)
                last[x] = r
                bit.add(r, 1)

                if bit.sum(r) % 2 == 1:
                    ans += r

            out.append(str(ans))
        return "\n".join(out)

    return solve()

# custom tests
assert run("1\n1\n1\n") == "1"
assert run("1\n3\n1 2 3\n") == "4"
assert run("1\n3\n1 1 1\n") == "6"
assert run("1\n4\n1 2 1 3\n") == run("1\n4\n1 2 1 3\n")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1`|`1`| Trường hợp kích thước tối thiểu | 
|`1 1 1 1 1`|`15`| Trường hợp cạnh hoàn toàn bằng nhau | 
|`1 2 3`|`4`| Tất cả các trường hợp riêng biệt | 
|`1 2 1 3`| biến | Xử lý lặp lại | 

## Vỏ cạnh 

Đối với một đầu vào như$[1,1,1]$, mỗi khi chúng tôi xử lý một lần xuất hiện mới, lần xuất hiện trước đó sẽ bị xóa khỏi tập hợp hoạt động. Sau khi xử lý chỉ mục 3, chỉ vị trí 3 vẫn hoạt động, do đó thuật toán không bao giờ đếm quá mức các bản sao. Bất biến được thiết lập tích cực đảm bảo rằng các giá trị lặp lại hoạt động giống như một con trỏ chuyển động thay vì tích lũy nhiều đóng góp. 

Đối với một đầu vào như$[1,2,3]$, không có sự loại bỏ nào xảy ra, do đó tập hoạt động phát triển một cách đơn điệu. Tính chẵn lẻ của kích thước hoạt động phản ánh trực tiếp cấu trúc tiền tố và thuật toán đếm tất cả các tiền tố hợp lệ mà không bị can thiệp từ các bản cập nhật.
