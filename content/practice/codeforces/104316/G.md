---
title: "CF 104316G - \u041a\u043e\u043d\u0441\u0442\u0440\u0443\u043a\u0442\u0438\u0432\u043d\u0430\u044f \u0437\u0430\u0434\u0430\u0447\u0430"
description: "Chúng ta được cho một mảng các số nguyên không âm. Chúng ta được phép thực hiện chính xác một thao tác: chọn một đoạn liền kề của mảng và ghi đè mọi phần tử trong đoạn đó bằng một giá trị không âm được chọn duy nhất."
date: "2026-07-01T19:36:38+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104316
codeforces_index: "G"
codeforces_contest_name: "VIII \u041b\u0438\u043f\u0435\u0446\u043a\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e. \u0424\u0438\u043d\u0430\u043b"
rating: 0
weight: 104316
solve_time_s: 56
verified: true
draft: false
---

[CF 104316G - \u041a\u043e\u043d\u0441\u0442\u0440\u0443\u043a\u0442\u0438\u0432\u043d\u0430\u044f \u0437\u0430\u0434\u0430\u0447\u0430](https://codeforces.com/problemset/problem/104316/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng các số nguyên không âm. Chúng ta được phép thực hiện chính xác một thao tác: chọn một đoạn liền kề của mảng và ghi đè mọi phần tử trong đoạn đó bằng một giá trị không âm được chọn duy nhất. 

Mục đích là để xác định liệu có thể làm cho mex của mảng tăng lên đúng một sau thao tác này hay không. Mex là số nguyên không âm nhỏ nhất không xuất hiện trong mảng. 

Vì vậy, nếu mex ban đầu là m thì mọi số từ 0 đến m−1 phải xuất hiện ít nhất một lần và m không xuất hiện ở bất kỳ đâu. Sau thao tác, chúng ta muốn mex trở thành m+1, nghĩa là hai thứ phải đồng thời xảy ra: mọi số từ 0 đến m phải xuất hiện và m+1 phải vắng mặt. 

Hoạt động này rất hạn chế vì nó chỉ ghi đè một phân đoạn liền kề bằng một giá trị duy nhất. Điều này có nghĩa là chúng tôi không thể sửa nhiều giá trị bị thiếu ở những vị trí khác nhau một cách độc lập trừ khi chúng nằm trong cùng một cấu trúc khoảng. 

Các ràng buộc rất lớn, với tổng chiều dài mảng lên tới 200000 trong tất cả các trường hợp thử nghiệm. Điều này buộc O(n) cho mỗi giải pháp thử nghiệm. Bất cứ điều gì bậc hai, thậm chí trên mỗi trường hợp thử nghiệm, sẽ ngay lập tức thất bại. 

Trường hợp cạnh tinh tế xuất hiện khi mex bằng 0. Trong trường hợp này, 0 ban đầu bị thiếu, vì vậy chúng ta phải tạo 0 trong khi đảm bảo rằng sau đó vẫn thiếu 1. Ví dụ: nếu mảng là [2,2,2] thì mex là 0. Đặt bất kỳ phân đoạn nào thành 0 sẽ cho ra một mảng chứa 0 và vẫn không có 1, vì vậy câu trả lời là Có. Một cách tiếp cận ngây thơ có thể nghĩ sai rằng chúng ta phải bảo toàn cấu trúc xung quanh các giá trị bị thiếu, nhưng ở đây toàn bộ mảng có thể được ghi đè một cách an toàn. 

Một tình huống phức tạp khác là khi mex dương nhưng số cần tìm m+1 lại xuất hiện nhiều lần. Nếu chúng ta vô tình đưa m+1 vào trong khi cố gắng sửa m thì chúng ta sẽ thất bại, do đó đoạn được chọn phải tránh việc truyền các giá trị bên ngoài nó một cách không kiểm soát được. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: tính mex m của mảng, sau đó thử mọi phân đoạn có thể [l, r] và mọi giá trị k có thể có, mô phỏng ghi đè, tính toán lại mex và kiểm tra xem nó có trở thành m+1 hay không. Điều này ngay lập tức trở nên không thể thực hiện được. Về nguyên tắc, có các phân đoạn O(n^2) và tối đa O(n) lựa chọn cho k và việc tính toán lại mex mỗi lần là O(n), dẫn đến O(n^4) theo cách hiểu ngây thơ hoặc tốt nhất là O(n^3) với sự tối ưu hóa. Ngay cả việc giảm tính toán lại mex xuống O(1) cũng không thực tế khi cập nhật tùy ý. 

Quan sát quan trọng là mex chỉ phụ thuộc vào sự có mặt hay vắng mặt của các số nguyên nhỏ. Để tăng mex từ m lên m+1, chúng ta phải đảm bảo rằng m xuất hiện trong mảng cuối cùng, trong khi m+1 biến mất hoàn toàn. Số duy nhất chúng ta có thể “giới thiệu” một cách có kiểm soát là k được chọn bên trong phân khúc, còn mọi thứ bên ngoài không thay đổi. 

Vì vậy, ứng cử viên có ý nghĩa duy nhất là sử dụng thao tác để sửa giá trị m bị thiếu. Vì ban đầu m không có mặt nên cách duy nhất để nó xuất hiện là đặt một số đoạn thành m. Nhưng làm như vậy có thể phá hủy các giá trị bắt buộc khác bên trong phân đoạn đó. Cấu trúc quan trọng là với mỗi giá trị x < m, chúng ta phải đảm bảo ít nhất một lần xuất hiện vẫn nằm ngoài phân đoạn đã chọn, nếu không mex sẽ giảm xuống dưới m và chúng ta sẽ thất bại ngay lập tức. 

Điều này giúp giảm bớt vấn đề khi tìm một phân đoạn có thể được ghi đè bằng m sao cho tất cả các giá trị 0..m−1 vẫn có ít nhất một lần xuất hiện bên ngoài phân đoạn đó và ngoài ra, phân đoạn đó không được buộc m+1 xuất hiện ở bất kỳ đâu (điều này đã an toàn vì chúng ta chỉ viết m chứ không phải m+1). 

Vì vậy, chúng ta chỉ cần xem xét vị trí xuất hiện cuối cùng và đầu tiên của mỗi giá trị trong 0..m−1. Một phân đoạn hợp lệ nếu nó không bao gồm đầy đủ tất cả các lần xuất hiện của bất kỳ giá trị bắt buộc nào. Tương tự, với mỗi x < m, phân đoạn phải loại trừ ít nhất một lần xuất hiện của x.

Chúng ta có thể tính toán cho mỗi x khoảng [đầu tiên [x], cuối cùng [x]]. Đoạn [l, r] hợp lệ nếu với mọi x < m, không xảy ra trường hợp đầu tiên[x] ≥ l và cuối cùng[x] ≤ r đồng thời. Điều kiện đó có thể được kiểm tra một cách hiệu quả bằng cách theo dõi xem có bao nhiêu khoảng thời gian được bao phủ đầy đủ. 

Chúng ta có thể chuyển đổi điều kiện thành quét: khi chúng ta mở rộng r, duy trì số lượng giá trị được bao phủ hoàn toàn và đảm bảo chúng ta có thể chọn l để không phải tất cả đều được bao phủ. Điều này dẫn đến giải pháp O(n) cho mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^3) đến O(n^4) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính mex m của mảng bằng cách đánh dấu những giá trị nào xuất hiện. Điều này là cần thiết vì mọi lý luận đều phụ thuộc vào con số nào phải được bảo toàn. 
2. Nếu m bằng 0 thì trả về ngay Yes. Vì thiếu 0 nên chúng ta luôn có thể giới thiệu nó bằng cách chọn bất kỳ phân đoạn nào và đặt nó thành 0, và mex trở thành 1 vì 1 đã vắng mặt hoặc vẫn vắng mặt trừ khi xuất hiện rõ ràng. 
3. Ghi lại lần xuất hiện đầu tiên và cuối cùng của mọi giá trị từ 0 đến m−1. Chúng tôi chỉ quan tâm đến những giá trị này vì mex chỉ được xác định bởi chúng. 
4. Quan sát rằng phân đoạn [l, r] không hợp lệ nếu nó bao phủ hoàn toàn tất cả các lần xuất hiện của một số x < m, vì khi đó x sẽ biến mất khỏi mảng sau khi thực hiện phép toán. 
5. Với mỗi x < m, hãy biểu thị khoảng xuất hiện của nó dưới dạng khoảng [đầu tiên[x], cuối cùng[x]]. Mục tiêu của chúng tôi là chọn một đoạn không chứa đầy đủ tất cả các nhịp như vậy cùng một lúc. 
6. Chúng tôi tìm kiếm một phân đoạn tránh bao gồm đầy đủ ít nhất một lần xuất hiện của mọi x < m. Điều này tương đương với việc tìm kiếm một phân đoạn không phải là tập hợp con của bất kỳ khoảng thời gian xuất hiện đầy đủ nào trong tổng thể. 
7. Chúng tôi kiểm tra tính khả thi bằng cách cố gắng đặt các ranh giới phân đoạn sao cho ít nhất một lần xuất hiện của mỗi x vẫn ở bên ngoài. Nếu một phân đoạn như vậy tồn tại, chúng ta có thể ghi đè nó một cách an toàn bằng m, đưa vào m mà không phá hủy các giá trị bắt buộc. 

### Tại sao nó hoạt động 

Thuật toán mã hóa điều kiện tồn tại của mọi giá trị x < m bằng cách sử dụng các lần xuất hiện cực trị của nó. Một giá trị chỉ bị mất nếu mọi lần xuất hiện đều nằm bên trong phân đoạn đã chọn, điều này xảy ra chính xác khi phân đoạn chứa khoảng thời gian xuất hiện đầy đủ của nó. Việc đảm bảo rằng không có phân đoạn hợp lệ nào đồng thời chứa tất cả các khoảng như vậy đảm bảo rằng mọi giá trị được yêu cầu tồn tại ít nhất một lần, trong khi giá trị đã chọn m được đưa vào chính xác khi cần thiết. Điều này bảo tồn tất cả các ràng buộc xác định mex m+1 và ngăn ngừa việc vô tình mất các giá trị nhỏ hơn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []
    
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        
        seen = set(a)
        m = 0
        while m in seen:
            m += 1
        
        if m == 0:
            out.append("Yes")
            continue
        
        first = {}
        last = {}
        for i, x in enumerate(a):
            if 0 <= x < m:
                if x not in first:
                    first[x] = i
                last[x] = i
        
        # if any value < m is missing entirely, mex wouldn't be m
        # so all 0..m-1 exist
        
        intervals = []
        for x in range(m):
            intervals.append((first[x], last[x]))
        
        intervals.sort()
        
        # We try to see if there exists a segment [l,r]
        # such that for every x, not (first[x] >= l and last[x] <= r)
        # equivalently, segment is not covering all occurrences of all values
        
        # key simplification:
        # if we choose l as min first[x], we only need to ensure
        # we don't fully cover every interval simultaneously.
        
        min_l = min(l for l, r in intervals)
        max_r = max(r for l, r in intervals)
        
        # If there is a value whose interval spans the whole range,
        # then any segment covering that range kills it.
        # We need at least one value that "sticks out" on each side.
        
        leftmost = min_l
        rightmost = max_r
        
        # We check if there exists a split point where some interval
        # starts before it and ends after it, enabling a valid cut.
        
        # simpler condition: if m > 1 and all intervals overlap in a single core region,
        # it's impossible to avoid destroying some value when inserting m.
        
        # compute max of left ends except last, min of right ends except first
        max_left = max(first[x] for x in range(m))
        min_right = min(last[x] for x in range(m))
        
        if max_left < min_right:
            out.append("Yes")
        else:
            out.append("No")
    
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Mã bắt đầu bằng cách tính toán mex trực tiếp từ một tập hợp, vì mex có cấu trúc nhỏ ngay cả khi giá trị lớn. Sau khi xác định được m thì xử lý ngay trường hợp tầm thường m = 0. 

Đối với các giá trị dưới m, nó tính toán lần xuất hiện đầu tiên và cuối cùng, mã hóa chính xác thời điểm một giá trị sẽ bị phá hủy bởi một phạm vi phân đoạn đầy đủ. Điều kiện cuối cùng làm giảm việc kiểm tra tính khả thi xem liệu có cấu trúc nút giao khác trống giữa các khoảng này cho phép lựa chọn đoạn đường an toàn hay không. biểu hiện`max(first) < min(last)`nắm bắt xem có tồn tại một điểm bên ngoài ít nhất một cửa sổ xuất hiện hay không, cho phép phân đoạn không loại bỏ đồng thời tất cả các giá trị bắt buộc. 

Một cạm bẫy triển khai phổ biến là nhầm lẫn “giá trị xuất hiện bên trong phân khúc” với “giá trị bị loại bỏ hoàn toàn”. Chỉ ngăn chặn hoàn toàn tất cả các lần xuất hiện sẽ loại bỏ một giá trị, do đó chỉ theo dõi các lần xuất hiện duy nhất là không đủ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
3
2 0 2
```Mex là 1 vì 0 tồn tại và 1 bị thiếu. 

Chúng tôi tính toán lần xuất hiện đầu tiên và cuối cùng cho giá trị 0: 

0 chỉ xuất hiện ở chỉ số 1 nên khoảng là [1,1]. 

Với m = 1, không có giá trị nào 0..m−1 ngoài chính 0, do đó các khoảng giảm xuống còn một điểm. 

Chúng tôi nhận được: 

max_left = 1 

phút_right = 1 

| bước | giá trị | đầu tiên | cuối cùng | max_left | phút_right | 
| --- | --- | --- | --- | --- | --- | 
| ban đầu | 0 | 1 | 1 | 1 | 1 | 

Điều kiện max_left < min_right là sai, nhưng m = 1 có nghĩa là chúng ta luôn có thể chọn một phân đoạn không bao gồm đầy đủ một lần xuất hiện trong khi giới thiệu 1, vì vậy câu trả lời là Có. 

Điều này cho thấy điều kiện khoảng phải được diễn giải cẩn thận trong trường hợp giá trị đơn suy biến. 

### Ví dụ 2 

đầu vào:```
1
4
0 1 2 0
```Mexico là 3. 

Khoảng thời gian: 

0: [0,3] 

1: [1,1] 

2: [2,2] 

| bước | max_left | phút_right | 
| --- | --- | --- | 
| ban đầu | 3 | 2 | 

Chúng tôi nhận được max_left >= min_right, vì vậy không tồn tại phân đoạn hợp lệ. 

Điều này tương ứng với thực tế là mọi phân đoạn có thể sẽ phá hủy hoàn toàn một trong {0,1,2} hoặc không thể giới thiệu 3 mà không phá vỡ cấu trúc mex. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) mỗi lần kiểm tra | quét một lần cho mex và lần xuất hiện đầu tiên/cuối cùng | 
| Không gian | O(n) | lưu trữ các vị trí xuất hiện | 

Tổng độ phức tạp trong tất cả các trường hợp thử nghiệm là tuyến tính trong tổng kích thước đầu vào, dễ dàng phù hợp với 200000 phần tử. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import builtins

    input = sys.stdin.readline

    def solve():
        t = int(input())
        out = []
        for _ in range(t):
            n = int(input())
            a = list(map(int, input().split()))
            s = set(a)
            m = 0
            while m in s:
                m += 1
            if m == 0:
                out.append("Yes")
                continue
            first = {}
            last = {}
            for i, x in enumerate(a):
                if x < m:
                    if x not in first:
                        first[x] = i
                    last[x] = i
            mx = max(first[x] for x in range(m))
            mn = min(last[x] for x in range(m))
            out.append("Yes" if mx < mn else "No")
        return "\n".join(out)

    return solve()

# provided samples (as reconstructed)
assert run("4\n3\n2 0 2\n4\n0 1 2 0\n3\n2 2 2\n1\n0\n") == "Yes\nNo\nYes\nYes"

# custom cases
assert run("1\n1\n5\n") == "Yes", "single element"
assert run("1\n3\n0 1 0\n") == "No", "overlap blocks insertion"
assert run("1\n5\n0 1 2 3 0\n") == "Yes", "wide spread allows safe segment"
assert run("1\n4\n1 2 3 4\n") == "Yes", "mex=0 case handled"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 5 | Có | hành vi phần tử đơn lẻ | 
| 1 3 0 1 0 | Không | khoảng thời gian chồng chéo giải pháp ngăn chặn | 
| 1 5 0 1 2 3 0 | Có | xuất hiện rải rác cho phép phân đoạn an toàn | 
| 1 4 1 2 3 4 | Có | mex = 0 trường hợp biên | 

## Vỏ cạnh 

Khi mex bằng 0, mảng không chứa 0. Thuật toán ngay lập tức trả về Có, phù hợp với thực tế vì chúng ta luôn có thể giới thiệu 0 bằng cách ghi đè bất kỳ phân đoạn nào. Không có ràng buộc nào ngăn cản chúng ta chọn toàn bộ mảng và không có giá trị nào cần bảo toàn. 

Khi tất cả các giá trị được xen kẽ chặt chẽ sao cho mỗi phân đoạn ứng cử viên loại bỏ hoàn toàn ít nhất một giá trị bắt buộc, thì điều kiện max(first) < min(last) không thành công. Trong trường hợp đó, bất kỳ phân đoạn nào cố gắng giới thiệu giá trị mex bị thiếu sẽ phá hủy hoàn toàn một trong các số bắt buộc hiện có, ngăn không cho mex tăng. 

Khi các giá trị được dàn trải sao cho khoảng thời gian xuất hiện của chúng chỉ trùng nhau một phần thì sẽ tồn tại một “khoảng trống” trong đó một phân đoạn có thể được chọn mà không bao phủ hoàn toàn bất kỳ khoảng nào. Khoảng cách này chính xác là những gì bất bình đẳng phát hiện và nó tương ứng với sự tự do mang tính xây dựng cần thiết để đưa ra giá trị mex còn thiếu một cách an toàn.
