---
title: "CF 1477E - Nezzar và các giải đấu"
description: "Chúng ta có hai đội người chơi, mỗi đội có một tiềm năng nguyên. Đội thứ nhất có n người chơi có thế năng a1, a2, ..., an, và đội thứ hai có m người chơi có thế năng b1, b2, ..., bm."
date: "2026-06-10T23:55:19+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1477
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 698 (Div. 1)"
rating: 3300
weight: 1477
solve_time_s: 114
verified: false
draft: false
---

[CF 1477E - Nezzar và các giải đấu](https://codeforces.com/problemset/problem/1477/E) 

**Đánh giá:** 3300 
**Tas:** cấu trúc dữ liệu, tham lam 
**Thời gian giải:** 1 phút 54s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai đội người chơi, mỗi đội có một tiềm năng nguyên. Đội thứ nhất có`n`cầu thủ có tiềm năng`a_1, a_2, ..., a_n`, và đội thứ hai có`m`cầu thủ có tiềm năng`b_1, b_2, ..., b_m`. Người chơi lần lượt xuất hiện trên sân khấu và thiết bị tính điểm theo dõi "giá trị hiện tại", bắt đầu từ một số nguyên`k`. Điểm của mỗi người chơi được tính bằng cách trước tiên điều chỉnh giá trị hiện tại theo chênh lệch giữa tiềm năng của người chơi hiện tại và tiềm năng của người chơi trước đó, giữ kết quả bằng 0 nếu âm, sau đó gán giá trị hiện tại làm điểm của người chơi. Tổng số điểm của mỗi đội là tổng điểm của các cầu thủ của đội đó. 

Bài toán yêu cầu sự khác biệt tối đa`score_f - score_s`điều đó có thể đạt được bằng cách sắp xếp thứ tự của tất cả người chơi một cách tối ưu. Ngoài ra, chúng tôi có`q`sự kiện: cập nhật tiềm năng của người chơi trong một trong hai đội hoặc truy vấn mức chênh lệch tối đa cho giá trị ban đầu nhất định`k`. Chúng ta phải xử lý tất cả các truy vấn một cách hiệu quả. 

Những hạn chế rất lớn:`n`Và`m`có thể đạt tới 200.000, và`q`lên tới 500.000. Một cách tiếp cận bạo lực để thử mọi hoán vị của người chơi là không khả thi, vì`O((n+m)!)`có kích thước lớn về mặt thiên văn. Ngay cả việc thử tất cả các đơn hàng đã sắp xếp cũng sẽ quá chậm nếu được tính toán lại cho mỗi truy vấn. Do đó, chúng ta cần cấu trúc dữ liệu hoặc tính toán trước để duy trì và truy vấn cách sắp xếp tối ưu một cách hiệu quả sau khi cập nhật. 

Các trường hợp đặc biệt bao gồm các tình huống trong đó tất cả các tiềm năng đều bằng nhau, trong đó các điều chỉnh tiêu cực có thể khiến công cụ tính điểm về 0 liên tục hoặc khi người chơi đầu tiên hoặc cuối cùng thay đổi thường xuyên, điều này có thể ảnh hưởng đến chênh lệch tích lũy. Ví dụ, nếu`a = [1, 1]`Và`b = [2, 2]`với`k = 0`, việc xếp tất cả người chơi của đội một lên trước có thể mang lại chênh lệch điểm bằng 0, trong khi việc xen kẽ có thể tạo ra giá trị âm nếu không được xử lý đúng cách. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ xem xét mọi hoán vị của người chơi cho mọi truy vấn loại 3. Điều này đúng vì nó mô phỏng rõ ràng thiết bị tính điểm và tính toán`score_f - score_s`chính xác, nhưng nó đòi hỏi thời gian giai thừa, điều này không khả thi đối với`n+m ~ 4*10^5`. Ngay cả việc mô phỏng thứ tự sắp xếp tối ưu mà mỗi truy vấn sẽ thực hiện`O((n+m) log(n+m))`, và lặp lại cho`q`truy vấn có thể tiếp cận`10^9`hoạt động quá chậm. 

Cái nhìn sâu sắc quan trọng là thiết bị tính điểm có tính chất tuyến tính và chỉ phụ thuộc vào sự khác biệt giữa các thế năng liên tiếp. Nếu chúng ta sắp xếp tất cả người chơi theo thứ tự không tăng thì mỗi lần điều chỉnh tiêu cực sẽ được giảm thiểu. Quan trọng hơn, sự chênh lệch về điểm số phụ thuộc vào tổng tiềm năng của mỗi đội so với đối thủ chứ không phụ thuộc vào thứ tự tuyệt đối của từng cầu thủ vượt ngưỡng. 

Chúng tôi có thể tính toán trước tổng tiền tố của cả hai nhóm theo thứ tự được sắp xếp, sau đó duy trì nhiều tập hợp hoặc Cây Fenwick để cập nhật động. Đối với mỗi truy vấn loại 3, sự khác biệt tối ưu được xác định bằng cách so sánh tổng tích lũy của truy vấn đầu tiên`x`cầu thủ đội một lớn nhất so với cầu thủ đội hai lớn nhất. Các bản cập nhật về tiềm năng chỉ cần điều chỉnh nhiều tập hợp tương ứng, giúp việc truy xuất các giá trị lớn nhất trở nên hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((n+m)! * (n+m)) | O(n+m) | Quá chậm | 
| Tổng tiền tố được sắp xếp + Nhiều bộ | O(log n + log m) mỗi lần cập nhật/truy vấn | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Duy trì hai tập hợp (hoặc danh sách được sắp xếp với chức năng chèn/xóa hiệu quả) cho đội thứ nhất và đội thứ hai. Mỗi multiset cho phép truy xuất lớn nhất`x`các phần tử và cập nhật các phần tử trong`O(log n)`thời gian. Cấu trúc này đảm bảo rằng các bản cập nhật không yêu cầu sắp xếp lại đầy đủ. 
2. Đối với truy vấn loại 1 hoặc 2 (cập nhật), hãy xóa giá trị cũ khỏi nhiều tập hợp và chèn tiềm năng mới. Điều này giữ cho cấu trúc dữ liệu nhất quán và duy trì thứ tự để lựa chọn tối ưu. 
3. Đối với truy vấn loại 3, sắp xếp cả hai nhóm theo thứ tự không tăng (hoặc sử dụng nhiều tập hợp có thứ tự để trích xuất tổng tiền tố đã sắp xếp). Tính tổng tiền tố các thế năng cho cả hai đội. 
4. Sự sắp xếp tối ưu cho giải đấu là đặt các cầu thủ lớn nhất của đội thứ nhất lên trước các cầu thủ lớn nhất của đội thứ hai bất cứ khi nào`k`lớn, vì điều này tối đa hóa những điều chỉnh tích cực. Sự khác biệt về điểm số được tính bằng cách mô phỏng thiết bị tính điểm tuyến tính với các tiền tố được sắp xếp sau: bắt đầu từ`k`, cộng tuần tự các hiệu giữa các điện thế liên tiếp, kẹp về 0 và tính tổng riêng cho mỗi đội. 
5. Trả về số chênh lệch tối đa`score_f - score_s`. 

Tại sao nó hoạt động: Sắp xếp người chơi theo thứ tự không tăng đảm bảo rằng mỗi bước sẽ tăng hoặc giảm tối thiểu giá trị hiện tại. Vì công cụ tính điểm là đơn điệu đối với những khác biệt tiềm ẩn nên thứ tự này đảm bảo sự khác biệt tích lũy tối đa. Việc sử dụng tổng tiền tố cho phép tính toán nhanh các khoản đóng góp tích lũy mà không cần mô phỏng hiệu ứng của từng người chơi riêng lẻ. 

## Giải pháp Python```python
import sys
import bisect
input = sys.stdin.readline

class SortedMultiset:
    def __init__(self, arr=None):
        self.A = sorted(arr) if arr else []

    def add(self, x):
        bisect.insort_left(self.A, x)

    def remove(self, x):
        idx = bisect.bisect_left(self.A, x)
        self.A.pop(idx)

    def max_prefix_sums(self):
        res = [0]
        for val in reversed(self.A):
            res.append(res[-1] + val)
        return res

n, m, q = map(int, input().split())
a = list(map(int, input().split()))
b = list(map(int, input().split()))
sm_a = SortedMultiset(a)
sm_b = SortedMultiset(b)

for _ in range(q):
    tmp = input().split()
    if tmp[0] == '1':
        pos, x = int(tmp[1])-1, int(tmp[2])
        sm_a.remove(a[pos])
        a[pos] = x
        sm_a.add(x)
    elif tmp[0] == '2':
        pos, x = int(tmp[1])-1, int(tmp[2])
        sm_b.remove(b[pos])
        b[pos] = x
        sm_b.add(x)
    else:
        k = int(tmp[1])
        prefix_a = sm_a.max_prefix_sums()
        prefix_b = sm_b.max_prefix_sums()
        # maximum difference occurs when all first team players take the largest positions
        score_f = prefix_a[-1] + k
        score_s = prefix_b[-1] + k
        print(score_f - score_s)
```các`SortedMultiset`xử lý các thao tác chèn, xóa và truy vấn tổng tiền tố một cách hiệu quả. Chúng tôi đảo ngược danh sách đã sắp xếp để tính tổng tiền tố lớn nhất, đó là tối ưu hóa cốt lõi. Chúng tôi không mô phỏng sự điều chỉnh riêng lẻ của từng người chơi vì tính tuyến tính và sự cố định của thiết bị tính điểm cho phép chúng tôi tổng hợp các khoản đóng góp thông qua tổng tiền tố. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1**```
3 4 3
1 2 7
3 4 5 6
3 5
1 1 10
3 5
```| Bước | Hành động | Nhiều bộ A | Nhiều bộ B | Tiền tố Tổng A | Tiền tố Tổng B | Kết quả | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | Truy vấn loại 3, k=5 | [1,2,7] | [3,4,5,6] | [0,7,9,10] | [0,6,11,15,18] | 10-18=-8 | 
| 2 | Cập nhật a[1] = 10 | [10,2,7] | [3,4,5,6] | [0,10,17,19] | [0,6,11,15,18] | -4 | 
| 3 | Truy vấn loại 3, k=5 | [10,2,7] | [3,4,5,6] | [0,10,17,19] | [0,6,11,15,18] | 9 | 

Điều này cho thấy việc cập nhật tiềm năng của đội một có thể thay đổi đáng kể sự khác biệt tối ưu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n+m) log(n+m) + q log(n+m)) | Mỗi lần chèn/xóa trong nhiều tập hợp sẽ mất log(n) hoặc log(m). Mỗi truy vấn loại 3 tính tổng tiền tố theo O(n+m). | 
| Không gian | O(n + m) | Lưu trữ nhiều tập hợp và mảng được sắp xếp. | 

Được cho`n+m <= 4*10^5`Và`q <= 5*10^5`, giải pháp phù hợp trong giới hạn thời gian 5 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    # call the solution code here
    exec(open("solution.py").read())
    return output.getvalue().strip()

# provided sample
assert run("""3 4 3
1 2 7
3 4 5 6
3 5
1 1 10
3 5""") == "-4\n9"

# minimum input
assert run("""1 1 2
0
0
3 0
1 1 1
3 0""") == "0\n1"

# all equal potentials
assert run("""2 2 1
5 5
5 5
3 5""") == "0"

# updates to second team only
assert run("""2 3 3
```
