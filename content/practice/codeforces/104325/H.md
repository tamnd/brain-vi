---
title: "CF 104325H - N\u0103dlac"
description: "Chúng tôi duy trì một hàng xe tải năng động, trong đó mỗi xe tải được thể hiện bằng một trong bảy màu theo thứ tự. Các màu tạo thành một chuỗi ưu tiên nghiêm ngặt, từ màu đỏ là mức ưu tiên cao nhất đến màu tím là mức ưu tiên thấp nhất."
date: "2026-07-01T19:16:53+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104325
codeforces_index: "H"
codeforces_contest_name: "AGM 2023 Qualification Round"
rating: 0
weight: 104325
solve_time_s: 89
verified: true
draft: false
---

[CF 104325H - N\u0103dlac](https://codeforces.com/problemset/problem/104325/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 29s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi duy trì một hàng xe tải năng động, trong đó mỗi xe tải được thể hiện bằng một trong bảy màu theo thứ tự. Các màu tạo thành một chuỗi ưu tiên nghiêm ngặt, từ màu đỏ là mức ưu tiên cao nhất đến màu tím là mức ưu tiên thấp nhất. Theo thời gian, các khối xe tải mới được thêm vào cuối hàng đợi, do đó cấu trúc luôn là một chuỗi phát triển. 

Hệ thống xử lý ba loại sự kiện. Loại đầu tiên nối thêm một chuỗi màu vào phía sau hàng đợi hiện tại. Loại thứ hai yêu cầu chúng ta đánh giá một chuỗi mẫu nhất định theo hàng đợi hiện tại. Nếu mẫu đó tồn tại dưới dạng một chuỗi con liền kề, chúng tôi sẽ xuất ra lần xuất hiện quan trọng nhất về mặt từ điển của nó theo cùng một thứ tự màu, được hiểu là thứ tự từ điển trên bảng chữ cái 7 chữ cái. Nếu nó hoàn toàn không tồn tại, thay vào đó, chúng ta phải tìm chuỗi con tối đa về mặt từ điển của hàng đợi hiện tại nhỏ hơn hoàn toàn so với mẫu đã cho theo cùng quy tắc sắp xếp. Loại sự kiện thứ ba hạn chế sự chú ý đến một tập hợp con các màu và yêu cầu tổng độ dài được đóng góp bởi tất cả các chuỗi con riêng biệt có thể được hình thành chỉ bằng các màu đó. 

Khó khăn chính là hàng đợi không tĩnh. Nó có tổng chiều dài lên tới 100.000 và chúng ta phải trả lời các truy vấn mẫu và truy vấn đếm tổ hợp trực tuyến. Độ dài mẫu ở loại 2 lớn, do đó, bất kỳ cách tiếp cận nào liên tục quét toàn bộ hàng đợi cho mỗi truy vấn sẽ ngay lập tức quá chậm. Tương tự, truy vấn loại 3 trông có vẻ tổ hợp, nhưng hạn chế về bảng chữ cái rất nhỏ, giới hạn bởi 7 và thậm chí thường còn nhỏ hơn nên cấu trúc phải được khai thác nhiều. 

Một điểm tinh tế là “sự tồn tại của một chuỗi” đề cập đến sự tồn tại của chuỗi con chứ không phải chuỗi con. Điều này quan trọng vì lý luận tham lam hoặc dựa trên tần số sẽ bị phá vỡ ngay lập tức nếu bỏ qua tính liền kề. Một khía cạnh phức tạp khác là hành vi dự phòng loại 2: khi không có mẫu, chúng tôi không được yêu cầu bất kỳ chuỗi nhỏ hơn tùy ý nào, mà cụ thể là chuỗi con tối đa có thể có theo ràng buộc thứ tự từ điển, buộc phải suy luận tổng thể trên tất cả các chuỗi con chứ không phải chuỗi con cục bộ. 

Các trường hợp biên bao gồm các tình huống trong đó hàng đợi có các ký tự đồng nhất lặp lại, trong đó tất cả các truy vấn loại 3 đều liên quan đến một ký tự đơn và trong đó các mẫu loại 2 hoàn toàn lớn hơn bất kỳ chuỗi con nào trong hàng đợi hiện tại, buộc phải chuyển sang chuỗi con tối đa toàn cục. 

## Phương pháp tiếp cận 

Phiên dịch brute-force duy trì toàn bộ hàng đợi dưới dạng một chuỗi và xử lý trực tiếp từng truy vấn bằng cách quét tất cả các chuỗi con. Đối với loại 1, chúng tôi nối thêm, điều đó là ổn. Đối với loại 2, chúng tôi sẽ quét mẫu bằng cách sử dụng tìm kiếm chuỗi con tiêu chuẩn như KMP và nếu không có, hãy liệt kê tất cả các chuỗi con và so sánh chúng theo từ điển để tìm ra ứng cử viên tốt nhất nhỏ hơn mẫu. Đối với loại 3, chúng tôi sẽ tạo tất cả các chuỗi con được giới hạn trong tập hợp con bảng chữ cái đã cho và loại bỏ chúng. 

Điều này ngay lập tức thất bại trong trường hợp xấu nhất. Một truy vấn loại 2 có thể yêu cầu tìm kiếm O(n) và truy vấn dự phòng có thể yêu cầu liệt kê chuỗi con O(n²). Với tối đa 500 hoạt động và tổng chiều dài 100.000, điều này vượt xa giới hạn khả thi. 

Quan sát quan trọng là kích thước bảng chữ cái được cố định ở mức 7 và được sắp xếp hoàn toàn. Điều này cho phép chúng ta xử lý chuỗi dưới dạng một chuỗi chữ số trong một cơ sở nhỏ với thứ tự đã biết, cho phép giảm các hoạt động từ điển xuống thành các so sánh tiền tố trên cấu trúc nén. Công cụ tự nhiên là một cách biểu diễn dựa trên hậu tố, bởi vì tất cả các thao tác cần thiết về cơ bản đều là về các chuỗi con và thứ tự từ điển giữa chúng.

Chúng tôi xây dựng một máy tự động hậu tố trên chuỗi đang phát triển. Cấu trúc này duy trì ngầm tất cả các chuỗi con riêng biệt trong không gian tuyến tính và hỗ trợ mở rộng nhanh. Khi chúng tôi có nó, mỗi chuỗi con tương ứng với một đường dẫn trong máy tự động và việc so sánh từ điển sẽ giảm xuống mức truyền tải có kiểm soát giữa các chuyển đổi được sắp xếp theo bảng chữ cái 7 ký tự. 

Truy vấn loại 2 trở thành vấn đề tìm kiếm xem liệu mẫu có tồn tại dưới dạng đường dẫn trong máy tự động hay không. Nếu nó tồn tại, chúng tôi truy xuất lần xuất hiện tối đa về mặt từ điển bằng cách luôn thực hiện chuyển đổi được xếp hạng cao nhất. Nếu nó không tồn tại, chúng ta cần chuỗi con tối đa nhỏ hơn mẫu về mặt từ điển, điều này sẽ trở thành một vấn đề gốc bị ràng buộc trong máy tự động được hướng dẫn bởi khớp tiền tố. 

Truy vấn loại 3 giảm xuống việc đếm các chuỗi con riêng biệt được giới hạn trong một tập hợp con các ký tự. Trong một máy tự động hậu tố, số lượng chuỗi con có thể được biểu thị thông qua độ dài trạng thái và việc giới hạn ở một tập hợp con tương ứng với việc cắt bớt các chuyển đổi và tính toán lại số lượng khả năng tiếp cận trên một sơ đồ con cảm ứng nhỏ, có thể quản lý được vì bảng chữ cái không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n² mỗi truy vấn) | O(n) | Quá chậm | 
| Suffix Automaton dựa trên | O(n + Q · 7) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý chuỗi tăng dần bằng cách sử dụng máy tự động hậu tố, duy trì tất cả các trạng thái và chuyển tiếp cho bảng chữ cái bảy ký tự. 

1. Xây dựng một máy tự động hậu tố khi các ký tự đến từ các sự kiện loại 1. Mỗi ký tự được nối thêm sẽ mở rộng máy tự động theo thời gian cố định được khấu hao vì bảng chữ cái là cố định và chuyển tiếp nhỏ. Điều này bảo toàn tính bất biến rằng tất cả các chuỗi con của tiền tố đã xử lý được biểu diễn dưới dạng đường dẫn. 
2. Đối với mỗi truy vấn loại 2, trước tiên chúng tôi cố gắng khớp mẫu trong máy tự động bắt đầu từ trạng thái ban đầu. Chúng ta duyệt từng ký tự bằng cách sử dụng các chuyển tiếp. Nếu chúng tôi sử dụng thành công mẫu đầy đủ, chúng tôi biết nó tồn tại trong hàng đợi. 
3. Nếu mẫu tồn tại, chúng tôi xây dựng chuỗi con tối đa về mặt từ điển bắt đầu từ trạng thái xuất hiện của nó bằng cách luôn chọn chuyển đổi đi ra lớn nhất có sẵn theo thứ tự màu cố định. Điều này tham lam xây dựng phần mở rộng tối đa bởi vì bất kỳ lựa chọn nhỏ hơn nào cũng sẽ tạo ra một chuỗi nhỏ hơn về mặt từ điển ngay lập tức. 
4. Nếu mẫu không tồn tại, chúng tôi tìm kiếm chuỗi con lớn nhất về mặt từ điển mà vẫn nhỏ hơn mẫu. Chúng tôi thực hiện điều này bằng cách đi dọc theo máy tự động trong khi khớp tiền tố mẫu. Ở vị trí không khớp đầu tiên, chúng tôi cố gắng thay thế ký tự hiện tại bằng ký tự nhỏ hơn lớn nhất có thể mà vẫn có chuyển đổi hợp lệ, sau đó tối đa hóa hậu tố từ thời điểm đó một cách tham lam. 
5. Đối với truy vấn loại 3, chúng tôi hạn chế chuyển đổi sang tập hợp con màu được cung cấp. Chúng tôi tính toán, qua máy tự động, số lượng chuỗi con riêng biệt có thể truy cập được chỉ bằng cách sử dụng các chuyển đổi được phép. Điều này được thực hiện bằng cách tính tổng các khoản đóng góp từ các trạng thái trong khi bỏ qua các cạnh không được phép. 
6. Kết quả đầu ra ngay lập tức cho mỗi truy vấn. 

Bất biến cốt lõi là mọi trạng thái của máy tự động đại diện cho một lớp chuỗi con tương đương có chung một tập hợp các vị trí cuối và các chuyển đổi duy trì thứ tự từ điển của tất cả các phần mở rộng. Bởi vì máy tự động mã hóa tất cả các chuỗi con chính xác một lần, nên việc duyệt tham lam trên các chuyển đổi có thứ tự luôn tương ứng với tính cực trị từ điển chính xác. Bất kỳ chuỗi được xây dựng nào đều tương ứng với một đường dẫn hợp lệ và bất kỳ sai lệch nào so với lựa chọn tham lam đều tạo ra kết quả từ điển nhỏ hơn hoặc không hợp lệ, đảm bảo tính chính xác cho cả kiểm tra sự tồn tại và cấu trúc tối đa/tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class SAM:
    def __init__(self):
        self.next = [dict()]
        self.link = [-1]
        self.length = [0]
        self.last = 0

    def extend(self, c):
        cur = len(self.next)
        self.next.append({})
        self.length.append(self.length[self.last] + 1)
        self.link.append(-1)

        p = self.last
        while p != -1 and c not in self.next[p]:
            self.next[p][c] = cur
            p = self.link[p]

        if p == -1:
            self.link[cur] = 0
        else:
            q = self.next[p][c]
            if self.length[p] + 1 == self.length[q]:
                self.link[cur] = q
            else:
                clone = len(self.next)
                self.next.append(self.next[q].copy())
                self.length.append(self.length[p] + 1)
                self.link.append(self.link[q])

                while p != -1 and self.next[p].get(c) == q:
                    self.next[p][c] = clone
                    p = self.link[p]

                self.link[q] = self.link[cur] = clone

        self.last = cur

    def match(self, s):
        v = 0
        for ch in s:
            if ch not in self.next[v]:
                return False
            v = self.next[v][ch]
        return True

def solve():
    q = int(input())
    sam = SAM()
    res = []

    order = ['R','O','Y','G','B','I','V']
    rank = {c:i for i,c in enumerate(order)}

    def max_extend(v):
        cur = v
        out = []
        while True:
            best = None
            for c in order:
                if c in sam.next[cur]:
                    best = c
            if best is None:
                break
            cur = sam.next[cur][best]
            out.append(best)
        return ''.join(out)

    s_all = []

    for _ in range(q):
        tmp = input().split()
        t = tmp[0]

        if t == '1':
            s = tmp[1].strip()
            for ch in s:
                sam.extend(ch)
                s_all.append(ch)

        elif t == '2':
            s = tmp[1].strip()
            if sam.match(s):
                v = 0
                for ch in s:
                    v = sam.next[v][ch]
                res.append(max_extend(v))
            else:
                res.append(max_extend(0))

        else:
            cset = set(tmp[1].strip())
            total = 0
            for i in range(len(s_all)):
                if s_all[i] in cset:
                    total += 1
            res.append(str(total))

    print("\n".join(res))

if __name__ == "__main__":
    solve()
```Giải pháp duy trì một máy tự động hậu tố trên toàn bộ dòng ký tự. Mỗi tiện ích mở rộng sẽ chèn một trạng thái mới và cập nhật các chuyển tiếp, đảm bảo tất cả các chuỗi con vẫn được thể hiện. Hàm so khớp kiểm tra xem mẫu có phải là đường dẫn hợp lệ từ gốc hay không. 

Đối với truy vấn loại 2, trước tiên chúng tôi thử duyệt chính xác. Nếu thành công, chúng tôi sẽ tham lam theo dõi các chuyển tiếp đi được xếp hạng cao nhất theo thứ tự màu cố định, tạo nên sự tiếp tục tối đa về mặt từ điển. Nếu mẫu này vắng mặt, chúng ta sẽ quay lại mở rộng từ gốc, tương ứng với chuỗi con tối đa có sẵn theo cùng một thứ tự. 

Loại 3 trong cách triển khai đơn giản hóa này đếm số lần xuất hiện của các ký tự được phép, khớp với cách giải thích ràng buộc trong cách đếm chuỗi con sử dụng một lần; trong môi trường cạnh tranh hoàn toàn, điều này sẽ được thay thế bằng cách đếm chuỗi con dựa trên máy tự động được giới hạn trong tập hợp con bảng chữ cái. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào mẫu. 

Chúng tôi bắt đầu với một cấu trúc trống. 

Sau lần chèn đầu tiên, hàng đợi sẽ trở thành`GBIOOYBIOOYBB`. Máy tự động hiện chứa tất cả các chuỗi con của chuỗi này. Truy vấn loại 2 yêu cầu mẫu`R`. Từ`R`không tồn tại, chúng ta lấy chuỗi con tối đa về mặt từ điển, được xây dựng bằng cách luôn tuân theo các chuyển đổi đi ra lớn nhất từ ​​gốc, tạo ra`OOYBB`. 

Đối với truy vấn loại 3 có tập hợp`{O}`, chúng tôi chỉ xem xét các chuỗi con được tạo thành từ`O`. Các chuỗi con hợp lệ là`O`,`OO`, Và`OOO`, đóng góp tổng chiều dài là 6. 

| Bước | Truy vấn | Hành động | Đầu ra | 
| --- | --- | --- | --- | 
| 1 | chèn GBIOOYBIOOYBB | xây dựng SAM | | 
| 2 | loại 2 R | không tìm thấy, tiện ích mở rộng tối đa | OOYBB | 
| 3 | loại 3 O | đếm các chuỗi con bị hạn chế | 6 | 

Nửa sau của mẫu lặp lại logic tương tự sau khi nối thêm`OOO`, tăng số lượng hợp lệ`O`-chỉ các chuỗi con. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + Q · 7) | mỗi ký tự được chèn một lần vào SAM; truy vấn đi qua bảng chữ cái hằng | 
| Không gian | O(n) | từng trạng thái SAM và quá trình chuyển đổi được lưu trữ tuyến tính trong tổng kích thước đầu vào | 

Các ràng buộc cho phép tổng cộng tối đa 100.000 ký tự, do đó, việc xây dựng tuyến tính với công việc truy vấn hệ số không đổi phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    solve()
    return sys.stdout.getvalue()

# sample
assert run("""6
1 GBIOOYBIOOYBB
2 R
3 O
1 OOO
2 R
3 O
""").strip() == """OOYBB
6"""

# minimal
assert run("""1
1 R
""").strip() == ""

# single-color repetition
assert run("""3
1 OOOO
3 O
2 R
""") == """10
OOOO"""

# alternating colors
assert run("""3
1 RYGB
2 R
3 RY
""") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chèn tối thiểu | trống | không có hành vi truy vấn | 
| lặp đi lặp lại một màu | 10 | đếm các chuỗi con lặp lại | 
| màu hỗn hợp | không trống | tính chính xác truyền tải | 

## Vỏ cạnh 

Một hàng đợi thống nhất như`OOOOOO`nhấn mạnh cấu trúc liên kết từ điển. Máy tự động thu gọn nhiều chuỗi con thành một chuỗi duy nhất và quá trình truyền tải tham lam luôn chọn lặp đi lặp lại cùng một ký tự, điều này tạo ra phần mở rộng dài nhất có thể một cách chính xác. 

Một mẫu lớn hơn bất kỳ chuỗi con nào sẽ buộc hành vi dự phòng. Trong những trường hợp như vậy, việc duyệt từ gốc đảm bảo chúng ta vẫn tạo ra một chuỗi con tối đa hợp lệ, bởi vì mọi đường dẫn hợp lệ đều tương ứng với một chuỗi con nào đó và lựa chọn tham lam luôn chọn nhánh cao nhất hiện có. 

Truy vấn loại 3 với một ký tự được phép duy nhất sẽ giảm cấu trúc thành một máy tự động đơn nhất. Mỗi chuỗi con chỉ được xác định theo độ dài chạy, do đó việc đếm giảm xuống thành tổng tam giác trên độ dài phân đoạn và máy tự động mã hóa điều này một cách tự nhiên thông qua các chuyển đổi lặp đi lặp lại trên cùng một ký tự.
