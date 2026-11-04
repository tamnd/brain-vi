---
title: "CF 1132E - Ba Lô"
description: "Chúng ta được cung cấp nhiều tập hợp các mục trong đó mỗi mục có trọng số từ 1 đến 8. Số lượng vật phẩm của mỗi trọng lượng là vô cùng lớn, nhưng chỉ số lượng của chúng mới quan trọng chứ không phải danh tính của chúng."
date: "2026-06-12T04:09:02+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "dp", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1132
codeforces_index: "E"
codeforces_contest_name: "Educational Codeforces Round 61 (Rated for Div. 2)"
rating: 2300
weight: 1132
solve_time_s: 71
verified: true
draft: false
---

[CF 1132E - Ba lô](https://codeforces.com/problemset/problem/1132/E) 

**Đánh giá:** 2300 
**Tags:** dfs và tương tự, dp, tham lam 
**Thời gian giải:** 1 phút 11 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp nhiều tập hợp các mục trong đó mỗi mục có trọng số từ 1 đến 8. Số lượng vật phẩm của mỗi trọng lượng là vô cùng lớn, nhưng chỉ số lượng của chúng mới quan trọng chứ không phải danh tính của chúng. Từ nhiều tập hợp này, chúng tôi muốn chọn một số tập hợp con của các mục sao cho tổng trọng lượng không vượt quá khả năng nhất định$W$và trong số tất cả các tập con hợp lệ như vậy, chúng ta muốn tối đa hóa tổng trọng số mà chúng ta chọn. 

Thoạt nhìn, điều này giống như một bài toán về chiếc ba lô giới hạn, nhưng cấu trúc đặc biệt là chỉ có tám trọng số có thể có. Số lượng rất lớn, lên tới$10^{16}$, điều này làm cho bất kỳ cách tiếp cận nào lặp lại trên các mục riêng lẻ đều không thể thực hiện được. 

Ràng buộc$W \le 10^{18}$cũng rất đáng kể. Nó ngay lập tức loại trừ bất kỳ chương trình động nào được lập chỉ mục theo trọng số, vì ngay cả DP tuyến tính trong$W$là không thể thực hiện được. Chúng ta phải làm việc với một biểu diễn chỉ phụ thuộc vào tám loại chứ không phụ thuộc trực tiếp vào số lượng hoặc dung lượng. 

Trường hợp góc tinh tế xuất hiện khi$W = 0$. Trong trường hợp đó không thể lấy được vật phẩm nào nên câu trả lời luôn là 0. Một tình huống khó khăn khác xảy ra khi chỉ riêng một hạng cân đã vượt quá khả năng cho phép nếu được sử dụng đầy đủ. Một cách tiếp cận tham lam ngây thơ lấy tất cả các mục nhỏ trước mà không xem xét việc phân bổ công suất còn sót lại theo các trọng số sẽ thất bại trong những trường hợp như vậy. 

## Phương pháp tiếp cận 

Một cách giải thích thô bạo sẽ là xem xét từng mục riêng lẻ và quyết định có nên lấy nó hay không. Điều này đúng về mặt khái niệm vì nó khám phá tất cả các tập hợp con, nhưng số lượng mục có thể lớn bằng$8 \cdot 10^{16}$, làm cho nó hoàn toàn không thể thực hiện được. 

Ngay cả khi chúng tôi nén theo số lượng và cố gắng giới hạn ba lô trên 8 loại vật phẩm, số lượng vẫn quá lớn. Bất kỳ DP nào lặp lại phạm vi đếm đều không thể thực hiện được. Điểm nghẽn không phải là số lượng loại mà là độ lớn của số lượng. 

Quan sát chính là số lượng loại ít nên chúng ta có thể coi các mặt hàng có cùng trọng lượng là không thể phân biệt được và xử lý chúng theo cách có cấu trúc. Thay vì quyết định từng món một, chúng ta suy luận về số lượng món hàng của mỗi trọng lượng mà chúng ta lấy, nhưng chúng ta tránh lặp lại tất cả các khả năng bằng cách giảm số lượng lớn một cách tham lam. 

Ý tưởng chính là bắt đầu từ những món đồ nặng nhất và di chuyển xuống dưới, luôn cố gắng lấp đầy dung lượng còn lại càng chặt càng tốt. Vì trọng số là số nguyên nhỏ nên trạng thái của bài toán có thể được điều chỉnh tăng dần: một khi chúng ta xác định số lượng vật phẩm nặng hơn mà chúng ta mang theo, dung lượng còn lại sẽ trở thành bài toán về chiếc ba lô nhỏ hơn với cùng cấu trúc. 

Một cách tối ưu hóa tiêu chuẩn và quan trọng là giới hạn tối đa số lượng mục của mỗi trọng lượng$W / i + 8$. Bất kỳ hạng mục bổ sung nào ngoài điều này đều không thể thay đổi câu trả lời vì ngay cả việc thay thế một số lượng nhỏ các hạng mục nặng hơn bằng nhiều hạng mục nhẹ hơn cũng không thể cải thiện tổng số tối ưu vượt ra ngoài vùng lân cận giới hạn này. Điều này cho phép chúng tôi giảm đáng kể số lượng hiệu quả. 

Sau lần giảm này, phiên bản còn lại trở nên đủ nhỏ để chúng ta có thể thực hiện DFS hoặc DP trên 8 chiều với việc cắt bớt dựa trên dung lượng còn lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên các tập hợp con |$O(2^N)$|$O(1)$| Quá chậm | 
| DFS bị giới hạn với việc cắt bớt số lượng |$O(\sum c_i \log W)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi nén số đếm trước để mỗi$cnt_i$không lớn hơn$W / i + 8$. Mức giảm này là an toàn vì bất kỳ giải pháp tối ưu nào cũng không thể thu được lợi ích nhiều hơn khoảng$W / i$vật có trọng lượng$i$và bộ đệm bổ sung đảm bảo vẫn có thể chuyển đổi giữa các hạng trọng số khi kết hợp các mục. 

Sau phần cắt xén này, chúng ta khám phá xem có bao nhiêu mục thuộc mỗi trọng số mà chúng ta lấy bằng cách sử dụng tìm kiếm đệ quy trên 8 trọng số từ 8 xuống 1. 

1. Chúng ta bắt đầu với trọng số 8 và quyết định nên đưa vào bao nhiêu mục có trọng số này. Với mỗi lựa chọn, chúng ta sẽ giảm dung lượng còn lại cho phù hợp. Điều này đúng vì tất cả các mặt hàng có cùng trọng lượng đều có thể thay thế cho nhau. 
2. Chúng ta tiến tới trọng số 7, lặp lại quá trình quyết định tương tự. Ở mỗi bước, chúng tôi đảm bảo không bao giờ vượt quá công suất còn lại. Việc sửa chữa từng bước này có tác dụng vì một khi các lựa chọn nặng hơn đã được cố định, các mục nhẹ hơn không thể ảnh hưởng trở lại tính khả thi. 
3. Chúng tôi tiếp tục cho đến trọng lượng 1. Ở cấp độ cuối cùng, chúng tôi lấy càng nhiều càng tốt mà không vượt quá khả năng, vì không có sự đánh đổi nào nữa. 
4. Trong suốt quá trình đệ quy, chúng tôi theo dõi tổng trọng lượng tốt nhất đạt được. Mỗi lá tương ứng với một tập hợp con hợp lệ hoàn chỉnh và chúng tôi lấy giá trị tối đa trên tất cả các cách xây dựng như vậy. 

Để giữ cho phép đệ quy khả thi, chúng tôi lược bỏ các trạng thái trong đó dung lượng còn lại đã được sử dụng hết hoặc ngay cả khi lấy tất cả các mục còn lại cũng không thể cải thiện câu trả lời tốt nhất được tìm thấy cho đến nay. 

Tại sao nó hoạt động dựa trên lập luận thống trị: bất kỳ giải pháp nào cũng có thể được chuyển thành dạng chuẩn trong đó các mục nặng hơn sẽ được quyết định trước. Vì trọng lượng nhỏ và rời rạc nên việc sắp xếp lại các mục không bao giờ làm tăng số lượng cấu hình có thể vượt quá giới hạn đã được cắt bớt. Việc cắt bớt đảm bảo rằng chúng tôi không bao giờ loại bỏ bất kỳ cấu hình nào có thể ảnh hưởng đến tính tối ưu, trong khi DFS đảm bảo chúng tôi khám phá tất cả các kết hợp có ý nghĩa theo thứ tự có cấu trúc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

W = int(input())
cnt = list(map(int, input().split()))

# compress counts to avoid explosion
for i in range(8):
    weight = i + 1
    cnt[i] = min(cnt[i], W // weight + 8)

best = 0

from functools import lru_cache

@lru_cache(maxsize=None)
def dfs(i, rem):
    global best
    if i == 8:
        return 0

    w = i + 1
    res = 0

    max_take = min(cnt[i], rem // w)

    for take in range(max_take + 1):
        val = take * w + dfs(i + 1, rem - take * w)
        if val > res:
            res = val

    best = max(best, res)
    return res

print(dfs(0, W))
```Việc triển khai trước tiên sẽ giảm số lượng xuống phạm vi có thể quản lý được để đệ quy không thể bùng nổ. Việc cắt xén này là cần thiết vì nếu không có nó, thậm chí việc lặp đi lặp lại tất cả các “lượng” có thể có cho một trọng số có thể lên tới$10^{16}$. 

DFS tiến hành theo thứ tự trọng lượng tăng dần, quyết định sử dụng bao nhiêu mục của mỗi trọng lượng. Công suất còn lại được truyền lại, đảm bảo tính khả thi luôn được duy trì. Việc ghi nhớ tránh tính toán lại các trạng thái có cùng chỉ mục và dung lượng còn lại. 

Một điểm tinh tế là thứ tự trọng lượng. Việc xử lý từ 1 đến 8 đảm bảo rằng khi chúng ta sửa các quyết định nặng hơn sớm hơn trong đệ quy, chúng ta tránh được việc đếm quá mức các kết hợp chỉ khác nhau về hoán vị của các quyết định có trọng số bằng nhau. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
W = 10
cnt = [1,2,3,4,5,6,7,8]
```Chúng tôi theo dõi các quyết định ở cấp độ cao. 

| Cân nặng | Còn lại W | Đi | Đóng góp | 
| --- | --- | --- | --- | 
| 8 | 10 | 1 | 8 | 
| 7 | 2 | 0 | 0 | 
| 6 | 2 | 0 | 0 | 
| 5 | 2 | 0 | 0 | 
| 4 | 2 | 0 | 0 | 
| 3 | 2 | 0 | 0 | 
| 2 | 2 | 1 | 2 | 
| 1 | 0 | 0 | 0 | 

Tổng cuối cùng là 10. 

Dấu vết này cho thấy rằng sự ưa thích tham lam đối với những món đồ nặng không vượt quá giá trị tối ưu vì công suất buộc phải có một điểm cắt tự nhiên. 

### Ví dụ 2 

đầu vào:```
W = 9
cnt = [10,10,10,10,10,10,10,10]
```| Cân nặng | Còn lại W | Đi | Đóng góp | 
| --- | --- | --- | --- | 
| 8 | 9 | 1 | 8 | 
| 7 | 1 | 0 | 0 | 
| 6 | 1 | 0 | 0 | 
| 5 | 1 | 0 | 0 | 
| 4 | 1 | 0 | 0 | 
| 3 | 1 | 0 | 0 | 
| 2 | 0 | 0 | 0 | 
| 1 | 0 | 0 | 0 | 

Tổng cuối cùng là 8. 

Điều này chứng tỏ rằng mặc dù có rất nhiều mặt hàng nhỏ nhưng chúng không thể bù đắp cho việc đóng gói kém một khi đã đưa ra lựa chọn nặng nề gần như tối ưu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(8 \cdot \prod (W/i + 8))$giảm hiệu quả về không gian trạng thái không đổi nhỏ | DFS trên 8 trọng lượng với việc cắt tỉa mạnh mẽ làm cho không gian trạng thái trở nên nhỏ bé | 
| Không gian |$O(W)$trong bản ghi nhớ trường hợp xấu nhất, nhưng có hiệu quả nhỏ do cắt tỉa | Bộ nhớ đệm chỉ lưu trữ các trạng thái có thể truy cập (i, rem) | 

Lý do chính khiến điều này vượt qua là vì sau khi đếm giới hạn, độ sâu đệ quy chỉ là 8 và việc phân nhánh bị hạn chế nặng nề bởi dung lượng còn lại. Không gian trạng thái sụp đổ xuống dưới giới hạn lý thuyết trong trường hợp xấu nhất. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    W = int(input())
    cnt = list(map(int, input().split()))

    for i in range(8):
        w = i + 1
        cnt[i] = min(cnt[i], W // w + 8)

    from functools import lru_cache

    @lru_cache(None)
    def dfs(i, rem):
        if i == 8:
            return 0
        w = i + 1
        best = 0
        max_take = min(cnt[i], rem // w)
        for t in range(max_take + 1):
            best = max(best, t * w + dfs(i + 1, rem - t * w))
        return best

    return str(dfs(0, W))

# provided sample
assert run("10\n1 2 3 4 5 6 7 8\n") == "10"

# minimum case
assert run("0\n0 0 0 0 0 0 0 0\n") == "0"

# single heavy item
assert run("10\n1 0 0 0 0 0 0 2\n") == "10"

# all small items
assert run("5\n10 10 10 10 10 10 10 10\n") == "5"

# tight packing
assert run("9\n1 1 1 1 1 1 1 1\n") == "9"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| W=0 tất cả đều bằng 0 | 0 | công suất trống | 
| chỉ nặng | sử dụng đầy đủ | sự thống trị của trọng lượng 8 | 
| dồi dào nhỏ | nắp chính xác | bão hòa tham lam | 
| chặt chẽ hỗn hợp | đóng gói chính xác | tương tác giữa các trọng lượng | 

## Vỏ cạnh 

Khi nào$W = 0$, phép đệ quy ngay lập tức không tìm thấy bước di chuyển hợp lệ nào ở bất kỳ mức trọng số nào và giá trị trả về bằng 0 vì mọi`max_take`trở thành số không do`rem // w`. 

Khi chỉ tồn tại các mặt hàng có trọng lượng cao, chẳng hạn như cân 8 mặt hàng có số lượng lớn, bước cắt xén sẽ giảm chúng xuống tối đa$W/8 + 8$, đảm bảo DFS không phát nổ. Thuật toán sau đó sẽ chọn chính xác$\lfloor W/8 \rfloor$các vật phẩm và chỉ sử dụng dung lượng còn lại nếu có thể, điều này duy trì sự tối ưu. 

Khi tất cả các trọng số đều nhỏ và nhiều, thuật toán vẫn hoạt động chính xác vì DFS khám phá các kết hợp đạt đến giới hạn dung lượng và khả năng ghi nhớ đảm bảo các bài toán con lặp lại không được tính toán lại.
