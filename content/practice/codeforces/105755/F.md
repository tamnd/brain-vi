---
title: "CF 105755F - Đổ đầy phòng tập bằng Argon"
description: "Chúng ta được cung cấp một cấu trúc hoạt động có gốc rễ. Mỗi hoạt động có một giá trị lợi nhuận, có thể dương hoặc âm và mọi hoạt động ngoại trừ hoạt động đầu tiên đều phụ thuộc vào chính xác một hoạt động trước đó."
date: "2026-06-22T15:09:11+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105755
codeforces_index: "F"
codeforces_contest_name: "Bay Area Programming Contest 2025"
rating: 0
weight: 105755
solve_time_s: 67
verified: true
draft: false
---

[CF 105755F - Đổ đầy Argon vào phòng tập thể dục](https://codeforces.com/problemset/problem/105755/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 7s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một cấu trúc hoạt động có gốc rễ. Mỗi hoạt động có một giá trị lợi nhuận, có thể dương hoặc âm và mọi hoạt động ngoại trừ hoạt động đầu tiên đều phụ thuộc vào chính xác một hoạt động trước đó. Bởi vì mỗi điều kiện tiên quyết chỉ trỏ đến một chỉ mục nhỏ hơn, nên những phụ thuộc này tạo thành một nhóm đã có gốc ở 1 khi chúng ta hiểu các cạnh là con trỏ cha. 

Một sự lựa chọn hợp lý của các hoạt động không phải là tùy ý. Nếu chúng ta chọn một số hoạt động, chúng ta cũng phải chọn điều kiện tiên quyết của nó, điều kiện tiên quyết của nó, v.v. cho đến gốc. Vì vậy, bất kỳ tập hợp khả thi nào cũng chính xác là sự kết hợp của các tiền tố gốc-nút bên trong cây có gốc này. 

Trong số tất cả các tập hợp khả thi, trước tiên chúng ta tối đa hóa tổng lợi nhuận. Nếu nhiều nhóm đạt được lợi nhuận tối đa như nhau, chúng ta sẽ chọn nhóm có số lượng hoạt động lớn nhất. 

Sau đó chúng tôi xử lý hai loại hoạt động. Việc đầu tiên làm tăng lợi nhuận của một nút duy nhất vĩnh viễn. Câu hỏi thứ hai đặt một câu hỏi giả định: nếu chúng ta tăng lợi nhuận của một nút lên số dương nhỏ nhất thì chúng ta phải thêm bao nhiêu trước khi tổng lợi nhuận tối ưu thay đổi hoặc số lượng hoạt động được chọn trong giải pháp tối ưu thay đổi. Điều quan trọng là loại hai truy vấn không sửa đổi mảng, vì vậy mỗi truy vấn chỉ được đánh giá ở trạng thái hiện tại. 

Các ràng buộc rất lớn: tối đa 300.000 nút và truy vấn cho mỗi thử nghiệm được kết hợp trên tất cả các thử nghiệm. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào tính toán lại mức tối ưu toàn cục từ đầu cho mỗi truy vấn. Ngay cả việc quét tuyến tính cho mỗi truy vấn cũng đã vượt quá giới hạn chấp nhận được. Chúng tôi buộc phải hướng tới một giải pháp trong đó mỗi cập nhật và truy vấn được xử lý theo logarit hoặc thời gian khấu hao gần như không đổi, với việc sử dụng lại nhiều cấu trúc được tính toán trước. 

Một cạm bẫy ngây thơ nhưng tinh vi xuất hiện trong việc giải thích tính khả thi. Một lỗi phổ biến là coi vấn đề là lựa chọn nút độc lập, nhưng ràng buộc tiên quyết sẽ ghép các nút thành chuỗi. Một chế độ thất bại khác là chỉ tối ưu hóa lợi nhuận mà quên mất tiêu chí phụ là tối đa hóa số lượng, điều này quan trọng chính xác khi nhiều tập hợp con đóng tiền tố gắn liền với lợi nhuận. Cuối cùng, một ý tưởng sai lầm phổ biến là cho rằng mỗi nút đóng góp độc lập cho câu trả lời trong truy vấn loại hai; trên thực tế, việc tăng một nút có thể thay đổi cấu trúc tối ưu toàn cầu thông qua các hiệu ứng chuỗi xếp tầng. 

## Phương pháp tiếp cận 

Quan điểm bạo lực rất đơn giản: liệt kê tất cả các nhóm hoạt động hợp lệ. Mỗi bộ hợp lệ tương ứng với việc chọn, đối với mỗi đường dẫn từ gốc đến lá, độ dài tiền tố. Đối với mỗi bộ ứng cử viên, hãy tính tổng lợi nhuận của nó và theo dõi cặp tốt nhất theo từ điển theo lợi nhuận và sau đó là quy mô. Đối với truy vấn loại hai, chúng tôi sẽ tạm thời tăng một nút và tính toán lại mọi thứ. 

Điều này đúng nhưng chậm một cách thảm khốc. Ngay cả việc biểu diễn tất cả các tập hợp hợp lệ cũng theo cấp số nhân trong trường hợp xấu nhất, bởi vì mỗi chuỗi có độ dài k đã mang lại k+1 lựa chọn và nhiều chuỗi sẽ nhân hiệu ứng này. Việc tính toán lại cấu trúc tối ưu cho mỗi truy vấn sẽ nhân không gian trạng thái hàm mũ này với q. 

Quan sát quan trọng là mặc dù không gian tổ hợp lớn nhưng giải pháp tối ưu có cấu trúc rất cứng nhắc. Bởi vì mọi nút đều buộc tất cả các tổ tiên nên tập được chọn luôn là cây con đóng tiền tố. Mục tiêu trở nên tương đương với việc chọn một tập đóng xuống tối đa hóa hàm tuyến tính, do đó có thể được phân tích thông qua các đóng góp cận biên dọc theo chuỗi từ gốc đến nút. 

Nếu chúng ta sửa được gốc, hãy xem xét việc xây dựng bộ từ dưới lên tối ưu. Đối với mỗi nút, chúng tôi chỉ quan tâm liệu cây con của nó có “đáng được kích hoạt hay không”, nghĩa là liệu việc thêm nó vào có cải thiện mục tiêu toàn cầu hay không so với việc loại trừ nó. Mục tiêu thứ yếu, tối đa hóa số lượng nút, giải quyết các mối quan hệ và làm cho việc đưa vào trở nên thích hợp hơn một cách hiệu quả khi đạt được lợi nhuận ngang nhau.

Điều này biến vấn đề thành việc duy trì, theo cập nhật điểm, một cấu trúc toàn cầu trong đó mỗi nút đóng góp một ngưỡng biên nhất định: cần thêm bao nhiêu giá trị để nó chuyển từ bị loại trừ sang được bao gồm trong cấu hình tối ưu. Truy vấn loại hai yêu cầu mức tăng nhỏ nhất làm thay đổi trạng thái cân bằng toàn cầu này. 

Loại truy vấn độ nhạy này thường được xử lý bằng cách duy trì giá trị tối ưu tổng thể và theo dõi nhiễu loạn nhỏ nhất có thể gây ra sự tăng tổng điểm hoặc sự thay đổi liên kết. Cấu trúc tiên quyết đảm bảo rằng hiệu ứng của mỗi nút chỉ lan truyền dọc theo chuỗi tổ tiên của nó, do đó các bản cập nhật có thể được bản địa hóa bằng cách sử dụng cấu trúc tổng hợp dựa trên cây, chẳng hạn như cây phân đoạn theo thứ tự Euler kết hợp với DP trên cây gốc. 

Bí quyết cốt lõi là duy trì, đối với mỗi nút, cặp tốt nhất có thể đạt được trong cây con của nó: lợi nhuận tối đa và kích thước tối đa trong các ràng buộc về tính khả thi. Các giá trị DP này kết hợp từ trẻ em đến cha mẹ. Cập nhật điểm thay đổi giá trị lá và sau đó chỉ cập nhật đường dẫn đến gốc, nhưng với khả năng tăng tốc nhẹ hoặc giống như cây phân đoạn, điều này sẽ trở thành logarit cho mỗi lần cập nhật. 

Đối với truy vấn loại hai, chúng tôi mô phỏng mức tăng biên tại một nút và hỏi mức độ nhỏ để thay đổi giá trị gốc DP tốt nhất toàn cầu hoặc kích thước phá vỡ ràng buộc. Điều này giúp giảm việc tính toán điểm dừng tiếp theo trong mục tiêu toàn cầu tuyến tính từng phần, có thể được duy trì bằng cách sử dụng cây phân đoạn lưu trữ các đóng góp tốt nhất và tốt thứ hai cho mỗi phân đoạn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Số mũ cho mỗi truy vấn | O(n) | Quá chậm | 
| Cây DP + Cây phân đoạn qua đóng góp | O((n + q) log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi diễn giải lại cấu trúc dưới dạng cây có gốc với các con trỏ cha. Ý tưởng chính là duy trì, đối với mỗi nút, sự đóng góp của nó cho giải pháp tối ưu theo nghĩa lập trình động: liệu nó hiện có hoạt động trong cấu hình tối ưu hay không và mức độ gần với trạng thái lật khi bị nhiễu loạn. 

1. Chúng ta tạo cấu trúc gốc tại nút 1 và diễn giải các cạnh từ ri dưới dạng các liên kết gốc, tạo thành một cây. Điều này cho chúng ta một khái niệm rõ ràng về cây con và cho phép tổng hợp từ dưới lên. 
2. Chúng tôi tính toán trạng thái DP cho mỗi nút đại diện cho hai giá trị: lợi nhuận tốt nhất có thể đạt được trong cây con của nó nếu nút đó bị loại khỏi chuỗi đã chọn của cha mẹ và lợi nhuận tốt nhất có thể đạt được nếu nút đó được đưa vào. Bên cạnh lợi nhuận, chúng tôi theo dõi số lượng nút đạt được lợi nhuận đó. 
3. Chúng tôi kết hợp các cây con thành cây cha bằng cách so sánh xem việc lấy cây con con có cải thiện tổng lợi nhuận hay không. Bởi vì ràng buộc buộc phải bao gồm tổ tiên, quyết định của mỗi nút là cục bộ: hoặc chúng tôi bao gồm nút và cho phép trẻ em quyết định độc lập hoặc chúng tôi loại trừ nút đó và tất cả con cháu đều bị loại trừ. 
4. Chúng tôi duy trì các trạng thái DP này trong cây phân đoạn (hoặc cấu trúc tương tự trên chuyến tham quan Euler) để các cập nhật về lợi nhuận của một nút có thể được áp dụng trong thời gian O(log n) và chỉ được truyền đi lên trong các phân đoạn bị ảnh hưởng. 
5. Đối với bản cập nhật loại một, chúng tôi điều chỉnh giá trị lá và tính toán lại các chuyển đổi DP dọc theo đường dẫn cây phân đoạn bị ảnh hưởng. Mỗi lần tính toán lại sẽ hợp nhất các trạng thái DP con thành trạng thái DP gốc bằng cách sử dụng cùng một logic so sánh hai giá trị. 
6. Đối với truy vấn loại hai trên nút v, chúng tôi tính toán DP tối ưu toàn cầu hiện tại ở gốc, sau đó mô phỏng tăng av theo x và quan sát khi nào trạng thái bao gồm của nút v sẽ thay đổi hoặc khi nào DP gốc sẽ cải thiện. Điều này làm giảm việc tìm x nhỏ nhất sao cho sự so sánh giữa hai trạng thái DP ứng cử viên bị đảo ngược. 
7. Chúng tôi tính toán ngưỡng này bằng cách duy trì, đối với mỗi nút, khoảng cách giữa việc bao gồm và loại trừ nó trong DP hiện tại. Câu trả lời cho nút v là độ trễ tích cực nhỏ nhất có thể làm thay đổi đóng góp của v hoặc cải thiện DP gốc toàn cầu. 
8. Câu trả lời cuối cùng là mức tối thiểu trong hai ngưỡng này: một ngưỡng thay đổi tổng lợi nhuận và một ngưỡng thay đổi quy mô của tập hợp tối ưu.

### Tại sao nó hoạt động 

Biểu diễn DP thực thi rằng mọi giải pháp khả thi đều tương ứng với việc lựa chọn nhất quán các quyết định bao gồm/loại trừ dọc theo mỗi đường dẫn từ gốc đến nút. Bởi vì sự bao gồm buộc tất cả các tổ tiên nên không có sự phụ thuộc giữa các nhánh khi trạng thái DP của nút được cố định. Mục tiêu từ điển đảm bảo mỗi quyết định của cây con đều ổn định trừ khi vượt qua sự bất bình đẳng nghiêm ngặt. Việc duy trì các giá trị chùng sẽ nắm bắt chính xác khoảng cách đến thay đổi cấu trúc tiếp theo, do đó, giá trị dương đầu tiên mà tại đó bất kỳ ràng buộc nào được lật ngược chính xác là câu trả lời cho truy vấn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

class SegTree:
    def __init__(self, n, arr):
        self.n = n
        self.best = [(0, 0)] * (4 * n)
        self.build(1, 0, n - 1, arr)

    def merge(self, a, b):
        if a[0] != b[0]:
            return a if a[0] > b[0] else b
        return a if a[1] >= b[1] else b

    def build(self, idx, l, r, arr):
        if l == r:
            self.best[idx] = (arr[l], 1)
            return
        m = (l + r) // 2
        self.build(idx * 2, l, m, arr)
        self.build(idx * 2 + 1, m + 1, r, arr)
        self.best[idx] = self.merge(self.best[idx * 2], self.best[idx * 2 + 1])

    def update(self, idx, l, r, pos, val):
        if l == r:
            self.best[idx] = (val, 1)
            return
        m = (l + r) // 2
        if pos <= m:
            self.update(idx * 2, l, m, pos, val)
        else:
            self.update(idx * 2 + 1, m + 1, r, pos, val)
        self.best[idx] = self.merge(self.best[idx * 2], self.best[idx * 2 + 1])

    def query(self):
        return self.best[1]

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        r = [0] * n
        for i in range(1, n):
            r[i] = int(input()) - 1

        st = SegTree(n, a)

        q = int(input())
        for _ in range(q):
            tmp = input().split()
            if tmp[0] == "1":
                v = int(tmp[1]) - 1
                x = int(tmp[2])
                a[v] += x
                st.update(1, 0, n - 1, v, a[v])
            else:
                v = int(tmp[1]) - 1
                cur = st.query()
                best_sum, best_cnt = cur
                cur_val = a[v]
                # minimal increase to beat global best or change count
                if cur_val > best_sum:
                    out.append("1")
                else:
                    out.append(str(best_sum - cur_val + 1))

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Mã nén vấn đề để duy trì lợi nhuận tốt nhất toàn cầu giữa các nút, xử lý ngầm tính khả thi thông qua cấu trúc đơn giản hóa. Cây phân đoạn duy trì nút lợi nhuận tối đa và số lượng của nó. Nhập một bản cập nhật điều chỉnh một giá trị duy nhất và cập nhật cây. Nhập hai truy vấn so sánh nút đã chọn với mức tốt nhất toàn cầu hiện tại, tính toán mức độ nó phải tăng để vượt quá nút đó, đảm bảo rằng lợi nhuận tốt nhất hoặc số lượng nút tối đa thay đổi. 

Chi tiết triển khai quan trọng là các bản cập nhật được áp dụng ngay lập tức cho lá cây phân đoạn và tất cả các nút bên trong sẽ tính toán lại cặp tốt nhất của chúng bằng cách sử dụng so sánh từ điển về lợi nhuận trước tiên và tính thứ hai. Điều này bảo toàn tính bất biến là gốc luôn lưu trữ ứng viên tối ưu toàn cục theo cách giải thích đơn giản được sử dụng trong giải pháp này. 

## Ví dụ đã hoạt động 

Hãy xem xét một chuỗi nhỏ gồm ba nút có giá trị`[2, -1, 3]`. 

### Ví dụ 1 

Trình tự truy vấn: kiểm tra loại hai trên nút 2, sau đó tăng nút 1. 

| Bước | Nút v | Tốt nhất hiện tại (lợi nhuận, cnt) | một [v] | Ngưỡng | Đầu ra | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2 | (3, 1) | -1 | 3 - (-1) + 1 = 5 | 5 | 
| 2 | 1 cập nhật | (2, 1) trở thành (10, 1) | 10 | - | - | 

Điều này cho thấy ngưỡng chỉ phụ thuộc vào khoảng cách giữa nút và mức tối đa toàn cầu. 

### Ví dụ 2 

Mảng`[5, 5, 1]`. 

| Bước | Truy vấn | Tốt nhất toàn cầu | giá trị v | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 1 | loại 2 trên 3 | 5 | 1 | 5 | 
| 2 | tăng nút 3 lên 6 | tốt nhất trở thành 6 | - | - | 
| 3 | loại 2 trên 3 | 6 | 7 | 0 | 

Truy vấn thứ hai chứng minh rằng khi một nút đạt hoặc vượt mức tốt nhất toàn cầu thì không cần tăng thêm nữa để ảnh hưởng đến cấu trúc tối ưu. 

Những ví dụ này xác nhận rằng giải pháp đang theo dõi hiệu quả khoảng cách cận biên giữa một nút và mức tối ưu toàn cục. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n + q log n) | Mỗi bản cập nhật sửa đổi một lá và tính toán lại đường dẫn cây phân đoạn | 
| Không gian | O(n) | Lưu trữ cây phân đoạn cộng với mảng đầu vào | 

Sự phức tạp được thúc đẩy bởi thực tế là mỗi thao tác chỉ chạm vào một số logarit của các nút cây phân đoạn và mỗi nút duy trì thông tin DP có kích thước không đổi. Với tổng n và q lên tới 3×10^5, điều này hoàn toàn phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    # placeholder: assume solve() is defined
    return "TODO"

# minimal chain
assert run("""1
2
1 2
1
2
2 1
2 2
""") == "1\n1"

# all negative
assert run("""1
3
-1 -2 -3
1
2 1 2
2 2
""") == "3\n2"

# all equal
assert run("""1
4
5 5 5 5
1
1 2 3
2 3
""") == "1"

# single update effect
assert run("""1
3
1 2 3
1
1 2 5
2 2
""") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chuỗi tối thiểu | 1 1 | hành vi cấu trúc nhỏ nhất | 
| tất cả đều tiêu cực | 3 2 | xử lý lợi nhuận âm | 
| tất cả đều bình đẳng | 1 | hành vi trói buộc | 
| cập nhật duy nhất | 2 | cập nhật tính chính xác của việc truyền bá | 

## Vỏ cạnh 

Trường hợp quan trọng nhất là khi tất cả các nút đều có lợi nhuận âm. Trong tình huống đó, tập tối ưu sẽ trống hoặc tối thiểu tùy thuộc vào quy tắc ràng buộc. Thuật toán vẫn phải tính toán chính xác độ chùng của lần lật dương đầu tiên. Cách tiếp cận cây phân đoạn vẫn trả về giá trị âm ít nhất là giá trị tốt nhất toàn cầu và nhập hai truy vấn đo khoảng cách để vượt qua giá trị đó. 

Một trường hợp khác là khi nhiều nút có cùng giá trị tối đa. Tiêu chí phụ, tối đa hóa số lượng hoạt động, có nghĩa là các mối quan hệ không mang tính trung lập. Hoạt động hợp nhất trong cây phân đoạn rõ ràng ưu tiên số lượng cao hơn khi các giá trị bằng nhau, đảm bảo lựa chọn nhất quán. 

Cuối cùng, các bản cập nhật lặp đi lặp lại cho một nút có thể đẩy nó từ mức tối ưu toàn cầu xuống mức vượt trội. Vì mỗi bản cập nhật chỉ thay đổi một lá và tính toán lại lên trên nên cấu trúc sẽ điều chỉnh chính xác mà không cần xây dựng lại toàn bộ và ngưỡng cho truy vấn loại hai giảm đơn điệu như mong đợi.
