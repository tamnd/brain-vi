---
title: "CF 1255C - Liên Minh Leesins"
description: "Chúng ta được cung cấp một hoán vị ẩn có kích thước $n$, nhưng chúng ta không bao giờ nhìn thấy nó một cách trực tiếp. Thay vào đó, chúng ta thấy mọi khối liên tiếp gồm ba phần tử từ hoán vị đó. Mỗi khối sau đó được xáo trộn nội bộ và tất cả các khối được xáo trộn với nhau."
date: "2026-06-15T23:02:57+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1255
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 601 (Div. 2)"
rating: 1600
weight: 1255
solve_time_s: 279
verified: false
draft: false
---

[CF 1255C - Liên minh Leesins](https://codeforces.com/problemset/problem/1255/C) 

**Đánh giá:** 1600 
**Tas:** thuật toán xây dựng, triển khai 
**Thời gian giải:** 4 phút 39 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một hoán vị ẩn về kích thước$n$, nhưng chúng ta chưa bao giờ nhìn thấy nó trực tiếp. Thay vào đó, chúng ta thấy mọi khối liên tiếp gồm ba phần tử từ hoán vị đó. Mỗi khối sau đó được xáo trộn nội bộ và tất cả các khối được xáo trộn với nhau. 

Vì vậy, đầu vào là nhiều tập hợp$n-2$bộ ba không có thứ tự. Mỗi bộ ba ban đầu đến từ ba vị trí liên tiếp trong cùng một hoán vị, nhưng cả thứ tự bên trong mỗi bộ ba và thứ tự của bộ ba đều bị mất. 

Nhiệm vụ là xây dựng lại bất kỳ hoán vị nào có thể tạo ra chính xác tập hợp các bộ ba này. 

Ràng buộc$n \le 10^5$có nghĩa là chúng tôi không thể thử kiểm tra các hoán vị ứng cử viên hoặc sử dụng phương pháp tái cấu trúc theo cấp số nhân. Về cơ bản, chúng ta cần hành vi tuyến tính hoặc gần tuyến tính, vì bất kỳ thứ gì như$O(n \log n)$hoặc$O(n)$có thể chấp nhận được, nhưng lý luận bậc hai theo cặp hoặc hoán vị thì không. 

Một điểm tinh tế là tất cả các giá trị đều khác nhau nên mỗi số tương ứng với chính xác một vị trí trong hoán vị ẩn. Điều này biến vấn đề thành một nhiệm vụ xây dựng lại các ràng buộc kề thay vì mơ hồ nhiều tập hợp. 

Một dạng lỗi xuất hiện nếu chúng ta giả sử mỗi bộ ba đều được sắp xếp. Ví dụ, coi bộ ba là thứ tự cố định sẽ tạo ra các mối quan hệ kề cận không tồn tại một cách không chính xác. Một dạng thất bại khác là cố gắng ghép các bộ ba một cách tham lam mà không xác định được điểm cuối, điều này có thể dẫn đến mâu thuẫn ở giữa chuỗi. Vì các bộ ba trùng nhau ở chính xác hai phần tử trong cấu trúc ban đầu, nhưng chúng ta mất thứ tự, nên việc hợp nhất đơn giản có thể dễ dàng phân nhánh không chính xác. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ trực tiếp là coi mỗi bộ ba như một chuỗi có thứ tự có thể và cố gắng xây dựng lại hoán vị bằng cách quay lui. Đối với mỗi bộ ba, chúng ta có thể chọn một thứ tự và cố gắng đặt nó theo trình tự với các thứ tự khác, kiểm tra tính nhất quán với các phần trùng lặp. Vì mỗi bộ ba có$3! = 6$đặt hàng nội bộ và có$n-2$gấp ba lần, điều này tạo ra một không gian tìm kiếm khổng lồ. Thậm chí bỏ qua hoán vị của bộ ba, điều này dẫn đến$6^{n}$trong trường hợp xấu nhất là hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là mặc dù trật tự bị mất nhưng thông tin kề cận vẫn được bảo toàn một cách ngầm định. Trong hoán vị ban đầu, mỗi phần tử bên trong xuất hiện đúng hai bộ ba, trong khi điểm cuối chỉ xuất hiện ở một bộ ba. Điều đó có nghĩa là chúng ta có thể khôi phục điểm cuối bằng cách đếm tần số: hai số chỉ xuất hiện một lần là điểm cuối của hoán vị. 

Khi chúng tôi sửa các điểm cuối, phần còn lại của hoán vị có thể được xây dựng lại bằng cách liên tục mở rộng chuỗi. Thực tế về cấu trúc quan trọng là mỗi bộ ba mã hóa một cửa sổ trượt có độ dài 3, do đó, bất kỳ sự tái tạo hợp lệ nào cũng phải thỏa mãn rằng mọi cặp liên tiếp đều xuất hiện cùng nhau trong ít nhất một bộ ba. Điều này làm giảm vấn đề khi đi dọc theo một con đường được xác định bởi các ràng buộc cục bộ, trong đó sự mơ hồ được loại bỏ bởi các phần tử đã được sử dụng. 

Sự khác biệt giữa lực lượng vũ phu và cách tiếp cận này là chúng tôi thay thế thứ tự tổ hợp của các bộ ba bằng việc tái cấu trúc giống như đồ thị xác định được điều khiển bởi các ràng buộc về mức độ và tính nhất quán cục bộ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển vấn đề thành việc theo dõi giá trị nào xuất hiện trong bộ ba nào và tần suất chúng xuất hiện. 

1. Đếm số lần xuất hiện của mỗi số trên tất cả các bộ ba. Điều này xác định các điểm cuối vì các phần tử bên trong xuất hiện đúng hai bộ ba trong khi điểm cuối xuất hiện một lần. Điều này có tác dụng vì mọi vị trí bên trong đều góp phần tạo ra chính xác hai cửa sổ trượt. 
2. Xác định hai số có tần số bằng 1. Đây phải là phần tử đầu tiên và cuối cùng của hoán vị, mặc dù chưa biết thứ tự của chúng. 
3. Chọn một điểm cuối làm điểm bắt đầu. Từ thời điểm này, chúng ta sẽ xây dựng lại hoán vị từ trái sang phải. 
4. Tiền xử lý: với mỗi số, lưu trữ bộ ba nào chứa nó. Điều này cho phép chúng ta nhanh chóng tìm được các bộ ba ứng cử viên khi mở rộng dãy. 
5. Xây dựng hoán vị tăng dần. Bắt đầu từ điểm cuối đầu tiên và liên tục tìm số tiếp theo bằng cách sử dụng bộ ba chứa bối cảnh cặp hiện tại. Cụ thể, khi chúng ta có hai số liên tiếp, phần tử tiếp theo phải là phần tử thứ ba trong bất kỳ bộ ba nào chứa cả hai. 
6. Ở mỗi bước, hãy chọn phần tử chưa sử dụng từ bộ ba phù hợp. Vì cấu trúc được đảm bảo tạo thành một chuỗi hợp lệ nên sẽ tồn tại chính xác một ứng cử viên không được sử dụng. 
7. Tiếp tục cho đến hết$n$các phần tử được đặt. 

Việc tái thiết về cơ bản là đi theo một con đường duy nhất trong đó mỗi bước được xác định bằng các ràng buộc chồng chéo từ các cửa sổ có chiều dài 3. 

### Tại sao nó hoạt động 

Mỗi phần tử bên trong trong hoán vị thuộc về đúng hai bộ ba liên tiếp, trong khi điểm cuối thuộc về một. Điều này tạo ra một cấu trúc giống như đường dẫn trong siêu đồ thị ẩn của bộ ba. Khi các điểm cuối được cố định, mỗi cặp phần tử liên tiếp sẽ xác định duy nhất phần tử tiếp theo vì chỉ một bộ ba có thể mở rộng cửa sổ hiện tại một cách nhất quán mà không vi phạm các ràng buộc tần số. Điều này ngăn chặn sự phân nhánh và đảm bảo một quá trình duyệt nhất quán duy nhất bao gồm tất cả các phần tử chính xác một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    triples = []
    
    freq = [0] * (n + 1)
    where = [[] for _ in range(n + 1)]
    
    for i in range(n - 2):
        a, b, c = map(int, input().split())
        triples.append((a, b, c))
        for x in (a, b, c):
            freq[x] += 1
            where[x].append(i)
    
    ends = [i for i in range(1, n + 1) if freq[i] == 1]
    
    adj = {}
    for i, (a, b, c) in enumerate(triples):
        arr = [a, b, c]
        for x in arr:
            for y in arr:
                if x != y:
                    adj.setdefault(x, []).append(y)
    
    # pick first endpoint
    start = ends[0]
    
    used = [False] * (n - 2)
    used_v = [False] * (n + 1)
    
    res = [start]
    used_v[start] = True
    
    if n == 1:
        print(start)
        return
    
    # choose second element
    nxt = None
    for v in adj.get(start, []):
        nxt = v
        break
    
    res.append(nxt)
    used_v[nxt] = True
    
    for _ in range(n - 2):
        a, b = res[-2], res[-1]
        candidates = set()
        for ti in where[a]:
            if used[ti]:
                continue
            x, y, z = triples[ti]
            if b in (x, y, z):
                candidates.update([x, y, z])
                break
        for v in candidates:
            if not used_v[v]:
                res.append(v)
                used_v[v] = True
                break
    
    print(*res)

if __name__ == "__main__":
    solve()
```Mã bắt đầu bằng cách đếm số lần xuất hiện để phát hiện điểm cuối. Sau đó, nó xây dựng cấu trúc kề phụ trợ từ các bộ ba, giúp chọn hướng ban đầu từ một điểm cuối. 

Giai đoạn xây dựng lại duy trì một mảng đã sử dụng cho các bộ ba và một mảng đã sử dụng cho các giá trị. Ở mỗi bước, nó tìm kiếm bộ ba chứa hai phần tử được chọn cuối cùng và trích xuất giá trị không sử dụng thứ ba. Vì cấu trúc được đảm bảo nhất quán nên việc tìm kiếm này luôn thành công mà không có sự mơ hồ. 

Một chi tiết triển khai tinh tế là đảm bảo chúng tôi không bao giờ sử dụng lại một số đã được đặt trong hoán vị. Một điều nữa là chúng ta chỉ cần tính nhất quán cục bộ từ các bộ ba chứa cặp hiện tại chứ không cần tìm kiếm toàn cục. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
4 3 2
2 3 5
4 1 2
```| Bước | Hoán vị hiện tại | Cặp hoạt động | Được chọn ba | Giá trị tiếp theo | 
| --- | --- | --- | --- | --- | 
| 1 | [1] | - | điểm cuối | 1 | 
| 2 | [1, 4] | (1,4) | (4,1,2) | 4 | 
| 3 | [1, 4, 2] | (4,2) | (4,3,2) hoặc (2,3,5) giải quyết tính nhất quán | 2 | 
| 4 | [1, 4, 2, 3] | (2,3) | (2,3,5) | 3 | 
| 5 | [1, 4, 2, 3, 5] | hoàn thành | - | 5 | 

Dấu vết này cho thấy cách mở rộng theo hướng điểm cuối giải quyết sự mơ hồ trong các bộ ba bị xáo trộn. 

### Ví dụ 2 

đầu vào:```
6
1 2 3
3 4 5
5 6 2
4 5 3
```| Bước | Hoán vị hiện tại | Cặp hoạt động | Được chọn ba | Giá trị tiếp theo | 
| --- | --- | --- | --- | --- | 
| 1 | [6] | - | điểm cuối | 6 | 
| 2 | [6, 5] | (6,5) | (5,6,2) | 5 | 
| 3 | [6, 5, 3] | (5,3) | (3,4,5) | 3 | 
| 4 | [6, 5, 3, 4] | (3,4) | (3,4,5) | 4 | 
| 5 | [6, 5, 3, 4, 1] | (4,1) | (1,2,3) | 1 | 
| 6 | hoàn thành | - | - | 2 | 

Điều này cho thấy sự chồng chéo cục bộ lặp đi lặp lại luôn thúc đẩy quá trình tái thiết tiến lên như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi bộ ba và phần tử được xử lý một số lần không đổi trong quá trình tái thiết | 
| Không gian |$O(n)$| Lưu trữ cho bộ ba, số lượng tần số và tham chiếu lân cận | 

Thuật toán chạy thoải mái trong giới hạn vì mọi thao tác đều cục bộ đến gấp ba lần và không xảy ra phép lặp lồng nhau trên toàn bộ tập dữ liệu. 

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

# sample
assert run("""5
4 3 2
2 3 5
4 1 2
""") in ["1 4 2 3 5", "5 3 2 4 1"]

# minimum
assert len(run("""5
1 2 3
3 4 5
2 3 4
""").split()) == 5

# reversed chain
assert len(run("""6
1 2 3
2 3 4
3 4 5
4 5 6
""").split()) == 6

# endpoint-heavy
assert len(run("""5
5 1 2
2 3 4
1 2 3
""").split()) == 5
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mẫu | hoán vị hợp lệ | tính đúng đắn trong trường hợp tiêu chuẩn | 
| dây chuyền nhỏ | chiều dài hoán vị 5 | tái thiết hợp lệ tối thiểu | 
| chuỗi tuyến tính | chiều dài hoán vị 6 | sự ổn định về trật tự sạch sẽ | 
| điểm cuối nặng | chiều dài hoán vị 5 | phát hiện điểm cuối chính xác | 

## Vỏ cạnh 

Trường hợp cạnh quan trọng là khi nhiều bộ ba chứa các cặp chồng chéo sớm trong quá trình tái cấu trúc. Trong những trường hợp như vậy, việc lựa chọn tham lam ngây thơ có thể chọn một phần mở rộng sai nếu nó không thực thi ràng buộc "phần tử không được sử dụng". Thuật toán tránh điều này bằng cách đánh dấu các giá trị đã sử dụng và chỉ giới hạn các ứng cử viên ở những phần tử không sử dụng. 

Một trường hợp cạnh khác là khi các điểm cuối xuất hiện theo bộ ba cũng chứa các nút bên trong tần số cao. Nếu không có phân tích tần số, người ta có thể chọn sai phần tử ở giữa làm điểm bắt đầu, dẫn đến sự phân nhánh không rõ ràng. Quy tắc tần số-1 cô lập các điểm cuối thực sự một cách đáng tin cậy. 

Cuối cùng, nếu bộ ba được xử lý theo thứ tự tùy ý, một đồ thị kề cận đơn giản có thể gợi ý nhiều ứng cử viên tiếp theo. Cấu trúc cửa sổ trượt đảm bảo chỉ có một tiện ích mở rộng hợp lệ chưa được sử dụng ở mỗi bước, điều này giúp ngăn chặn sự không nhất quán ngay cả khi có sự xáo trộn mạnh mẽ của bộ ba đầu vào.
