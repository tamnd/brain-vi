---
problem: 963D
contest_id: 963
problem_index: D
name: "Frequency of String"
contest_name: "Tinkoff Internship Warmup Round 2018 and Codeforces Round 475 (Div. 1)"
rating: 2500
tags: ["hashing", "string suffix structures", "strings"]
answer: passed_samples
verified: true
solve_time_s: 109
date: 2026-06-18
model: gpt-5-3-mini
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a33a36d-603c-83ec-9659-5838d7876b75
---

#CF 963D - Tần số của chuỗi 

**Đánh giá:** 2500 
**Tas:** băm, cấu trúc hậu tố chuỗi, chuỗi 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 1 phút 49s 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a33a36d-603c-83ec-9659-5838d7876b75 

--- 

##Giải pháp 

## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi văn bản cố định và sau đó là nhiều truy vấn mẫu độc lập. Mỗi truy vấn đưa ra một chuỗi mẫu và tần số bắt buộc. Đối với mỗi truy vấn, chúng ta cần xem xét tất cả các chuỗi con của chuỗi chính có chứa mẫu đủ thường xuyên và trong số các chuỗi con đó, hãy chọn chuỗi con ngắn nhất. Nếu mẫu không xuất hiện đủ số lần trong chuỗi chính trên toàn cầu thì câu trả lời là không thể. 

Đối tượng chính không chỉ là liệu một mẫu có xuất hiện hay không mà còn là nơi xuất hiện của nó nằm bên trong chuỗi chính. Khi chúng ta biết tất cả các vị trí bắt đầu của một mẫu, mọi chuỗi con hợp lệ sẽ được xác định bằng cách chọn một khối xuất hiện liên tiếp và kéo dài cửa sổ từ lần xuất hiện đầu tiên trong khối đến lần xuất hiện cuối cùng trong khối. Câu trả lời là khoảng nhỏ nhất có thể mà vẫn chứa ít nhất k lần xuất hiện. 

Các ràng buộc buộc phải thực hiện chiến lược tiền xử lý toàn cục. Tổng độ dài mẫu tối đa là 10^5 và có tối đa 10^5 truy vấn, do đó, việc tính toán lại kết quả khớp mẫu cho mỗi truy vấn sẽ vượt quá cả thời gian và bộ nhớ. Bất kỳ giải pháp nào quét chuỗi chính cho mỗi truy vấn đều dẫn đến O(n^2) trong trường hợp xấu nhất và thất bại ngay lập tức. 

Trường hợp cạnh tinh tế xuất hiện khi mẫu dài nhưng xuất hiện theo cách chồng chéo, ví dụ như trong một chuỗi như "aaaaa". Số lần xuất hiện của "aaa" trùng lặp nhiều và khoảng thời gian tối thiểu phụ thuộc vào cách tính các lần trùng lặp là các lần xuất hiện riêng biệt. Một vấn đề khác nảy sinh khi k bằng 1, trong đó câu trả lời chỉ đơn giản là độ dài mẫu nếu nó tồn tại, nhưng các cách tiếp cận ngây thơ giả sử các lần xuất hiện rời rạc vẫn có thể vượt quá hoặc đặt sai ranh giới. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp sẽ xử lý từng truy vấn riêng biệt bằng cách quét chuỗi chính và thu thập tất cả các lần xuất hiện của mẫu. Đối với mỗi mẫu, chúng tôi lấy vị trí xuất hiện của nó và tính toán cửa sổ tối thiểu bao gồm k lần xuất hiện liên tiếp. Điều này đúng, vì bất kỳ chuỗi con nào chứa k lần xuất hiện đều phải bao gồm k lần xuất hiện liên tiếp theo thứ tự xuất hiện. Vấn đề là chi phí: quét toàn bộ chuỗi cho mỗi truy vấn sẽ mang lại O(|s| * n) trong trường hợp xấu nhất, tức là khoảng 10^10 thao tác và không khả thi. 

Quan sát quan trọng là tất cả các mẫu đều được biết trước và tổng chiều dài của chúng nhỏ. Điều này gợi ý việc xây dựng một cấu trúc toàn cầu có thể trả lời “mẫu này xuất hiện ở đâu trong s” cho nhiều mẫu cùng một lúc. Máy tự động hậu tố hoặc mảng hậu tố có LCP sẽ hoạt động, nhưng sự phù hợp tự nhiên nhất ở đây là máy tự động hậu tố được tăng cường bằng tính năng theo dõi vị trí cuối. 

Một máy tự động hậu tố biểu diễn gọn gàng tất cả các chuỗi con của s và cho phép chúng ta xác định trạng thái tương ứng với bất kỳ mẫu nào trong O(|pattern|). Mỗi trạng thái đại diện cho một tập hợp các lần xuất hiện của chuỗi con và bằng cách truyền bá các vị trí cuối thông qua các liên kết hậu tố, chúng ta có thể thu thập tất cả các chỉ số xuất hiện cho từng mẫu phù hợp. Khi chúng ta có tất cả các lần xuất hiện, chúng ta chỉ cần tính toán cửa sổ tốt nhất bao gồm k vị trí được sắp xếp liên tiếp, đây là mức tối thiểu của cửa sổ trượt đơn giản trên một danh sách được sắp xếp. 

Do đó, giải pháp sẽ trở thành: xây dựng SAM, cung cấp từng chuỗi truy vấn thông qua nó, truy xuất tất cả các vị trí cuối của trạng thái đó, sắp xếp chúng và tính toán khoảng thời gian tối thiểu bao gồm k lần xuất hiện liên tiếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Quét lực lượng vũ phu cho mỗi truy vấn | O( | s | · n) | 
| Hậu tố automaton + danh sách xuất hiện | O( | s | + Σ | 

## Hướng dẫn thuật toán

Chúng tôi xây dựng một máy tự động hậu tố trên chuỗi chính. Mỗi trạng thái lưu trữ các chuyển tiếp theo ký tự, liên kết hậu tố và độ dài. Ngoài ra, mỗi trạng thái lưu trữ một danh sách các vị trí cuối nơi xuất hiện các chuỗi con của trạng thái đó. Chúng tôi điền các danh sách này trong quá trình xây dựng bằng cách đánh dấu trạng thái cuối của từng tiền tố và truyền bá các lần xuất hiện theo thứ tự giảm dần về độ dài trạng thái. 

Đối với mỗi mẫu truy vấn, chúng tôi sẽ hướng dẫn từng ký tự máy tự động. Nếu ở bất kỳ bước nào chúng ta không thể theo dõi quá trình chuyển đổi thì mẫu đó không tồn tại và câu trả lời là -1. 

Nếu chúng ta đạt đến một trạng thái thành công, trạng thái đó đại diện cho tất cả các lần xuất hiện của mẫu. Chúng tôi trích xuất danh sách xuất hiện của nó, chứa vị trí kết thúc của tất cả các kết quả khớp trong chuỗi. Từ mỗi vị trí kết thúc, chúng ta rút ra vị trí bắt đầu của sự xuất hiện mẫu. 

Chúng tôi sắp xếp các vị trí bắt đầu này. Sau đó chúng tôi quét một cửa sổ có kích thước k trên danh sách đã sắp xếp này. Đối với mỗi cửa sổ, chúng tôi tính toán khoảng chuỗi con từ lần xuất hiện đầu tiên đến lần xuất hiện cuối cùng. Câu trả lời tốt nhất là khoảng thời gian tối thiểu như vậy trên tất cả các cửa sổ. 

Cuối cùng, chúng tôi xuất độ dài tối thiểu này hoặc -1 nếu có ít hơn k lần xuất hiện. 

### Tại sao nó hoạt động 

Bất kỳ chuỗi con nào chứa k lần xuất hiện của một mẫu sẽ tạo ra k vị trí bắt đầu lần xuất hiện bên trong nó và các vị trí này phải xuất hiện theo thứ tự tăng dần. Chuỗi con chặt chẽ nhất bao gồm k lần xuất hiện luôn được xác định bởi một số khối k lần xuất hiện liên tiếp theo thứ tự được sắp xếp, vì việc bỏ qua một lần xuất hiện chỉ làm tăng khoảng cách. Máy tự động đảm bảo chúng tôi liệt kê chính xác tất cả các lần xuất hiện và cửa sổ trượt đảm bảo chúng tôi kiểm tra tất cả các nhóm liên tiếp tối ưu mà không bỏ sót bất kỳ cấu hình nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class SAM:
    def __init__(self, n):
        self.next = [dict()]
        self.link = [-1]
        self.length = [0]
        self.pos = [[]]
        self.last = 0

    def extend(self, c, idx):
        cur = len(self.next)
        self.next.append({})
        self.length.append(self.length[self.last] + 1)
        self.link.append(0)
        self.pos.append([idx])

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
                self.pos.append([])

                while p != -1 and self.next[p].get(c) == q:
                    self.next[p][c] = clone
                    p = self.link[p]

                self.link[q] = self.link[cur] = clone

        self.last = cur

    def build(self, s):
        for i, ch in enumerate(s):
            self.extend(ch, i)

    def propagate(self):
        order = sorted(range(len(self.next)), key=lambda x: self.length[x], reverse=True)
        for v in order:
            if self.link[v] != -1:
                self.pos[self.link[v]].extend(self.pos[v])

    def match(self, t):
        v = 0
        for ch in t:
            if ch not in self.next[v]:
                return None
            v = self.next[v][ch]
        return v

s = input().strip()
sam = SAM(len(s))
sam.build(s)
sam.propagate()

q = int(input())
for _ in range(q):
    k, m = input().split()
    k = int(k)

    v = sam.match(m)
    if v is None:
        print(-1)
        continue

    occ = sam.pos[v]
    if len(occ) < k:
        print(-1)
        continue

    starts = [p - len(m) + 1 for p in occ]
    starts.sort()

    ans = 10**18
    for i in range(len(starts) - k + 1):
        l = starts[i]
        r = starts[i + k - 1] + len(m) - 1
        ans = min(ans, r - l + 1)

    print(ans)
```Máy tự động hậu tố được xây dựng tăng dần trên chuỗi, với mỗi trạng thái ghi lại một vị trí cuối cho mỗi lần nó được tạo. Sau khi xây dựng, danh sách xuất hiện được truyền dọc theo các liên kết hậu tố sao cho mỗi trạng thái tích lũy tất cả các vị trí cuối cùng của chuỗi con mà nó đại diện. Hàm khớp thực hiện các chuyển đổi để xác định trạng thái tương ứng với mẫu. 

Một chi tiết triển khai tinh tế là việc truyền bá phải diễn ra theo thứ tự độ dài giảm dần, nếu không thì việc tổng hợp liên kết hậu tố sẽ mất tính chính xác vì các trạng thái ngắn hơn phụ thuộc vào các trạng thái dài hơn. Một chi tiết quan trọng khác là chuyển đổi vị trí cuối thành vị trí bắt đầu một cách cẩn thận với`p - len(m) + 1`, vì SAM lưu trữ các chỉ số kết thúc. 

Tính toán cửa sổ cuối cùng giả định các lần xuất hiện đã được sắp xếp; vì việc truyền bá SAM không đảm bảo trật tự nên cần phải sắp xếp rõ ràng. 

## Ví dụ đã hoạt động 

Hãy xem xét chuỗi mẫu`aaaaa`và mẫu truy vấn`aaa`với k = 2. 

Sau khi lan truyền, vị trí kết thúc lần xuất hiện là`[2, 3, 4]`. 

| Cửa sổ | Lần xuất hiện (kết thúc) | Vị trí bắt đầu | Khoảng cách | 
| --- | --- | --- | --- | 
| [2,3] | 2,3 | 0,1 | 3 | 
| [3,4] | 3, | | |
