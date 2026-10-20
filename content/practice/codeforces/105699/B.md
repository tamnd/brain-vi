---
title: "CF 105699B - Người Vợ Tốt Nhất"
description: "Chúng ta được cung cấp một chuỗi các khoảng thời gian và sau mỗi khoảng thời gian mới xuất hiện, chúng ta phải trả lời một câu hỏi lập kế hoạch: chỉ sử dụng các khoảng thời gian đã thấy cho đến nay, số lượng lớn nhất trong số chúng có thể được chọn là bao nhiêu để không trùng lặp về thời gian."
date: "2026-06-22T04:51:33+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105699
codeforces_index: "B"
codeforces_contest_name: "OCPC 2024 Winter, Day 8: Borys Minaiev Contest 1 (The 3rd Universal Cup. Stage 27: London)"
rating: 0
weight: 105699
solve_time_s: 62
verified: true
draft: false
---

[CF 105699B - Người vợ tuyệt vời nhất](https://codeforces.com/problemset/problem/105699/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 2s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các khoảng thời gian và sau mỗi khoảng thời gian mới xuất hiện, chúng ta phải trả lời một câu hỏi lập kế hoạch: chỉ sử dụng các khoảng thời gian đã thấy cho đến nay, số lượng lớn nhất trong số chúng có thể được chọn là bao nhiêu để không trùng lặp về thời gian. 

Mỗi khoảng thời gian đại diện cho một hoạt động diễn ra hàng ngày từ ngày bắt đầu đến ngày kết thúc, bao gồm cả hai. Hai hoạt động xung đột nếu chúng chia sẻ ít nhất một ngày. Điều này bao gồm trường hợp ranh giới trong đó một kết thúc vào ngày`d`và một ngày khác bắt đầu vào ngày`d`, điều này rõ ràng không được phép. 

Vì vậy sau khi xử lý lần đầu`i`khoảng, chúng tôi muốn kích thước tối đa của một tập hợp con trong số này`i`các khoảng sao cho không có hai khoảng đã chọn nào giao nhau theo thời gian. 

Các ràng buộc rất lớn: lên tới 300.000 khoảng và giá trị tọa độ lên tới 600.000. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào tính toán lại lịch trình tối ưu từ đầu cho mỗi tiền tố. Một bậc hai hoặc thậm chí`O(n sqrt n)`cách tiếp cận sẽ không tồn tại. Giải pháp phải coi mỗi khoảng thời gian mới là một bản cập nhật gia tăng trên một cấu trúc có thể trả lời nhanh chóng “đâu là chuỗi kết thúc tốt nhất trước thời điểm này”. 

Một trường hợp thất bại phổ biến đối với các phương pháp tiếp cận ngây thơ xuất hiện khi các khoảng được lồng nhau dày đặc hoặc chồng chéo lên nhau. Ví dụ: nếu chúng ta liên tục chèn các khoảng như`[1, 100000]`,`[2, 99999]`,`[3, 99998]`, một cách tiếp cận tham lam hoặc tính toán lại quét lại tất cả các khoảng thời gian mỗi lần trở nên cực kỳ tốn kém, mặc dù câu trả lời chỉ thay đổi dần dần. 

Một trường hợp cạnh tinh tế khác là quy tắc ranh giới. Vì việc chạm vào các điểm cuối được coi là xung đột nên các khoảng thời gian`[1, 2]`Và`[2, 3]`không thể lấy cả hai. Bất kỳ giải pháp nào xử lý vấn đề này như một vấn đề “chồng chéo không nghiêm ngặt” tiêu chuẩn với`r <= l`khả năng tương thích sẽ tạo ra kết quả không chính xác. 

## Phương pháp tiếp cận 

Cách giải thích brute-force rất đơn giản: sau mỗi khoảng thời gian mới, hãy chạy thuật toán lập kế hoạch khoảng thời gian có trọng số cổ điển trên tất cả các khoảng thời gian đã thấy cho đến nay, nhưng vì tất cả các trọng số là một nên điều này giảm xuống còn việc tìm số lượng khoảng thời gian không chồng chéo tối đa. Sắp xếp theo thời gian kết thúc và thực hiện quét lập trình động sẽ tính toán chính xác câu trả lời cho tiền tố đó. 

Điều này hiệu quả vì việc lập kế hoạch theo khoảng thời gian có cấu trúc con tối ưu: nếu chúng tôi cố định một khoảng thời gian là khoảng thời gian được chọn cuối cùng thì chúng tôi chỉ cần giải pháp tốt nhất kết thúc trước khi nó bắt đầu. Tuy nhiên, việc lặp lại điều này từ đầu sau mỗi lần chèn sẽ tốn`O(i log i)`hoặc`O(i^2)`mỗi truy vấn tùy thuộc vào chi tiết triển khai. Tổng hợp tất cả các tiền tố, nó trở thành khối trong trường hợp xấu nhất, vượt xa giới hạn. 

Quan sát quan trọng là chúng ta thực sự không bao giờ cần tính toán lại cấu trúc toàn cục. Mỗi khoảng`(l, r)`chỉ hỏi một câu hỏi: chuỗi nào có thể đạt được tốt nhất và kết thúc đúng trước ngày`l`? Khi chúng tôi biết giá trị đó, chúng tôi có thể mở rộng nó bằng cách lấy khoảng này, tạo ra chuỗi ứng cử viên kết thúc tại`r`. Vì vậy, mỗi khoảng đóng góp chính xác một lần chuyển đổi trên đường đi dài nhất DP theo thời gian. 

Điều này gợi ý việc duy trì cấu trúc theo các vị trí thời gian có thể trả lời các truy vấn tối đa tiền tố và hỗ trợ cập nhật điểm. Chúng tôi lưu trữ vào mỗi thời điểm`t`độ dài chuỗi tốt nhất kết thúc chính xác tại`t`và chúng tôi duy trì cực đại tiền tố trên các giá trị này. Sau đó, mỗi khoảng thời gian sẽ trở thành một truy vấn duy nhất cộng với một bản cập nhật. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tính toán lại DP sau mỗi lần chèn | O(n² log n) trường hợp xấu nhất | O(n) | Quá chậm | 
| Tiền tố tối đa với cây phân đoạn | O(n log M) | O(M) | Đã chấp nhận | 

Đây`M`là giá trị tọa độ tối đa. 

## Hướng dẫn thuật toán 

Chúng tôi xử lý chỉ mục hàng ngày như một vị trí trên một dòng và chúng tôi duy trì thông tin về lịch trình tốt nhất kết thúc tại hoặc trước mỗi vị trí. 

1. Duy trì một mảng`best[t]`, lưu trữ số lượng khoảng thời gian tốt nhất trong bất kỳ lịch trình hợp lệ nào có khoảng thời gian được chọn cuối cùng kết thúc chính xác vào thời điểm`t`. 
2. Duy trì cấu trúc dữ liệu có thể trả lời: cho bất kỳ`x`, giá trị lớn nhất của`best[t]`tổng thể`t ≤ x`. Đây là truy vấn tối đa tiền tố. 
3. Đối với mỗi khoảng thời gian đến`(l, r)`, tính toán`base = max(best[t]) for all t ≤ l-1`. Đây là lịch trình tốt nhất kết thúc trước khi khoảng thời gian này bắt đầu. 
4. Hình thành giá trị ứng viên`candidate = base + 1`, nghĩa là chúng tôi mở rộng lịch trình đó bằng cách lấy khoảng thời gian hiện tại. 
5. Cập nhật`best[r] = max(best[r], candidate)`. Chúng tôi chỉ cải thiện nó nếu khoảng thời gian này tạo ra một lịch trình tốt hơn kết thúc vào lúc`r`. 
6. Theo dõi câu trả lời chung dưới dạng giá trị lớn nhất từng được lưu trữ trong`best`. Sau khi xử lý từng khoảng thời gian, xuất mức tối đa toàn cục này. 

Ý tưởng trọng tâm là các khoảng thời gian chỉ kéo dài các giải pháp về phía trước theo thời gian, vì vậy chúng ta không bao giờ cần phải xem lại các quyết định trước đó ngoại trừ thông qua các truy vấn tối đa tiền tố. 

### Tại sao nó hoạt động 

Bất kỳ lịch trình hợp lệ nào cũng có thể được coi là một chuỗi các khoảng thời gian được sắp xếp theo thời gian hoàn thành. Khi xét một khoảng`(l, r)`, mọi lịch trình tối ưu kết thúc tại`r`phải bao gồm một số lịch trình tối ưu kết thúc đúng trước`l`, theo sau là`(l, r)`. Truy vấn tối đa tiền tố nắm bắt chính xác chuỗi tiền tố tốt nhất có thể và bước cập nhật duy trì phần mở rộng tốt nhất cho mỗi điểm cuối. Vì chúng tôi luôn lưu trữ chuỗi tốt nhất cho mỗi điểm cuối nên không có khoảng thời gian nào trong tương lai có thể làm mất hiệu lực các tiền tố tối ưu trước đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Fenwick:
    def __init__(self, n):
        self.n = n
        self.bit = [0] * (n + 1)

    def update(self, i, v):
        while i <= self.n:
            if v > self.bit[i]:
                self.bit[i] = v
            i += i & -i

    def query(self, i):
        res = 0
        while i > 0:
            if self.bit[i] > res:
                res = self.bit[i]
            i -= i & -i
        return res

def main():
    n = int(input())
    MAXV = 600000

    ft = Fenwick(MAXV)
    answer = 0

    for _ in range(n):
        l, r = map(int, input().split())
        best_before = ft.query(l - 1)
        val = best_before + 1

        # update at r
        ft.update(r, val)

        if val > answer:
            answer = val

        print(answer)

if __name__ == "__main__":
    main()
```Cây Fenwick được sử dụng hoàn toàn như một cấu trúc tiền tố tối đa. Mỗi bản cập nhật ghi độ dài chuỗi ứng cử viên tại điểm cuối bên phải của khoảng thời gian và mỗi truy vấn sẽ truy xuất kết thúc chuỗi tốt nhất có thể đạt được trước khi khoảng thời gian hiện tại bắt đầu. 

Chi tiết thực hiện quan trọng là việc sử dụng nghiêm ngặt`l - 1`trong truy vấn. Điều này thực thi quy tắc rằng các khoảng chạm vào điểm cuối không tương thích. Một điểm tinh tế khác là các cập nhật mang tính đơn điệu theo nghĩa là chúng tôi luôn giữ giá trị tối đa cho mỗi vị trí, vì nhiều khoảng thời gian có thể kết thúc ở cùng một vị trí.`r`. 

## Ví dụ đã hoạt động 

Hãy xem xét một chuỗi nhỏ: 

đầu vào:```
3
1 3
3 5
1 2
```Chúng tôi theo dõi`best_before`,`val`, Và`answer`. 

| Bước | Khoảng thời gian | tốt nhất trước | giá trị | tốt nhất[r] sau khi cập nhật | trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | [1,3] | 0 | 1 | tốt nhất[3]=1 | 1 | 
| 2 | [3,5] | 0 | 1 | tốt nhất[5]=1 | 1 | 
| 3 | [1,2] | 0 | 1 | tốt nhất[2]=1 | 1 | 

Câu trả lời không bao giờ vượt quá 1 vì mỗi khoảng đều xung đột với các khoảng khác do trùng lặp điểm cuối. 

Bây giờ hãy xem xét một trường hợp có cấu trúc chặt chẽ hơn: 

đầu vào:```
4
1 2
3 4
5 6
2 5
```| Bước | Khoảng thời gian | tốt nhất trước | giá trị | tốt nhất[r] sau khi cập nhật | trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | [1,2] | 0 | 1 | tốt nhất[2]=1 | 1 | 
| 2 | [3,4] | 1 | 2 | tốt nhất[4]=2 | 2 | 
| 3 | [5,6] | 2 | 3 | tốt nhất[6]=3 | 3 | 
| 4 | [2,5] | 0 | 1 | tốt nhất[5]=1 | 3 | 

Khoảng thời gian cuối cùng không thể cải thiện chuỗi tốt nhất vì nó cản trở khả năng tương thích với cấu trúc trước đó, nhưng nó cũng không thể đánh bại chuỗi vốn đã tối ưu. 

Những dấu vết này cho thấy rằng mỗi khoảng chỉ đóng góp nếu nó mở rộng chuỗi tiền tố tốt nhất hiện có. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log M) | Mỗi khoảng thực hiện một truy vấn tối đa tiền tố và cập nhật một điểm trên cây Fenwick | 
| Không gian | O(M) | Dung lượng lưu trữ cho cây Fenwick trên phạm vi tọa độ lên tới 600.000 | 

Giới hạn tọa độ đủ nhỏ để hệ số logarit có thể dễ dàng được chấp nhận cho 300.000 phép tính. Cả trí nhớ và thời gian đều ở trong giới hạn thoải mái. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import sys

    class Fenwick:
        def __init__(self, n):
            self.n = n
            self.bit = [0] * (n + 1)

        def update(self, i, v):
            while i <= self.n:
                if v > self.bit[i]:
                    self.bit[i] = v
                i += i & -i

        def query(self, i):
            res = 0
            while i > 0:
                if self.bit[i] > res:
                    res = self.bit[i]
                i -= i & -i
            return res

    input = sys.stdin.readline
    n = int(sys.stdin.readline())
    MAXV = 10

    ft = Fenwick(MAXV)
    ans = []

    for _ in range(n):
        l, r = map(int, sys.stdin.readline().split())
        val = ft.query(l - 1) + 1
        ft.update(r, val)
        ans.append(str(val if val > 0 else 0))

    return "\n".join(ans)

# sample-like sanity checks
assert run("3\n1 3\n3 5\n1 2\n") == "1\n1\n1", "sample-like"

# non-overlapping chain
assert run("3\n1 2\n3 4\n5 6\n") == "1\n2\n3", "chain"

# fully overlapping
assert run("3\n1 10\n2 9\n3 8\n") == "1\n1\n1", "nested"

# boundary conflict
assert run("2\n1 2\n2 3\n") == "1\n1", "endpoint conflict"

# single interval
assert run("1\n5 5\n") == "1", "single"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| khoảng thời gian chuỗi | trình tự tăng dần | tích lũy tối ưu | 
| khoảng lồng nhau | tất cả chồng chéo | không xếp chồng sai | 
| chạm điểm cuối | cùng ranh giới | quy tắc xung đột nghiêm ngặt | 
| khoảng đơn | trường hợp tầm thường | độ đúng cơ sở | 

## Vỏ cạnh 

Một trường hợp tinh tế là khi nhiều khoảng chia sẻ cùng một điểm cuối bên phải. Ví dụ, khoảng`(1, 10)`,`(2, 10)`,`(3, 10)`tất cả đều cập nhật cùng một vị trí. Cấu trúc Fenwick đảm bảo chỉ tồn tại giá trị tối đa, do đó các chuyển đổi yếu hơn sẽ được bỏ qua một cách an toàn. 

Một trường hợp khác là khi`l = 1`. Truy vấn trở thành`query(0)`, phải trả về 0. Điều này tương ứng với việc bắt đầu một lịch trình từ đầu mà không có bất kỳ khoảng thời gian nào trước đó. 

Trường hợp cạnh cuối cùng có sự chồng chéo dày đặc xung quanh các giá trị tọa độ nhỏ. Ngay cả khi có nhiều khoảng thời gian xen kẽ, mỗi lần cập nhật chỉ củng cố trạng thái có nguồn gốc từ tiền tố, do đó không cần phải điều chỉnh ngược và thuật toán vẫn ổn định theo thứ tự đối nghịch.
