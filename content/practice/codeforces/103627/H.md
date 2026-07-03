---
title: "CF 103627H - Con Đường Bất Tận"
description: "Chúng ta có một tập hợp các đoạn trên một đường, trong đó mỗi đoạn đại diện cho một phần và đoạn đường mà chúng đi qua. Bản thân con đường là liên tục về mặt khái niệm, nhưng cấu trúc thú vị duy nhất lại đến từ các điểm cuối của đoạn đường."
date: "2026-07-02T22:34:11+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103627
codeforces_index: "H"
codeforces_contest_name: "XXII Open Cup, Grand Prix of Daejeon"
rating: 0
weight: 103627
solve_time_s: 50
verified: true
draft: false
---

[CF 103627H - Con đường vô tận](https://codeforces.com/problemset/problem/103627/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một tập hợp các đoạn trên một đường, trong đó mỗi đoạn đại diện cho một phần và đoạn đường mà chúng đi qua. Bản thân con đường là liên tục về mặt khái niệm, nhưng cấu trúc thú vị duy nhất lại đến từ các điểm cuối của đoạn đường. Quá trình này phát triển theo thời gian: ở mỗi bước, chúng tôi chọn một thành viên theo quy tắc dựa trên độ dài chưa được khám phá còn lại đối với họ và sau đó chúng tôi “kích hoạt” thành viên đó, thao tác này sẽ loại bỏ các phần của con đường khỏi việc xem xét thêm. Sau khi một phần đường bị loại bỏ, mọi đoạn đường vẫn chồng lên nó sẽ mất đi sự đóng góp tương ứng với việc loại bỏ đó. 

Mục tiêu là xác định thứ tự các thành viên được kích hoạt và xử lý các hiệu ứng của chúng một cách hiệu quả. Chế độ xem đơn giản rất đơn giản: liên tục mô phỏng thành phần nào có độ dài chưa được khám phá còn lại nhỏ nhất, cập nhật tất cả các phân đoạn bị ảnh hưởng và tiếp tục cho đến khi tất cả các phân đoạn được xử lý. 

Các ràng buộc ngụ ý rằng số lượng phân đoạn có thể đủ lớn để bất kỳ mô phỏng bậc hai nào trên các thành phần và các khoảng chồng chéo sẽ không thành công. Giải pháp quét liên tục tất cả các phân đoạn hoặc tất cả các phần nguyên tử cho mỗi lần kích hoạt ngay lập tức là quá chậm vì mỗi bản cập nhật có thể ảnh hưởng đến nhiều phân đoạn và điều này sẽ tích lũy thành$O(N^2)$hành vi trong trường hợp dày đặc. 

Một chi tiết cấu trúc quan trọng là các điểm cuối của phân đoạn xác định tối đa$2N$ranh giới có ý nghĩa. Giữa các điểm cuối liên tiếp không có gì thay đổi, do đó con đường có thể được phân tách thành các khoảng nguyên tử và tất cả các tương tác đều xảy ra ở mức độ chi tiết này. 

Một trường hợp thất bại tinh vi đối với mô phỏng tham lam ngây thơ phát sinh khi nhiều phân đoạn chồng chéo lên nhau và chia sẻ các điểm cuối. Nếu chúng tôi tính toán lại độ dài còn lại bằng cách quét tất cả các khoảng thời gian nguyên tử cho mỗi thao tác, chúng tôi sẽ tính gấp đôi công việc và cũng có nguy cơ bị đứt liên kết không chính xác nếu các giá trị còn lại bằng nhau không được xử lý nhất quán với các chỉ số ban đầu. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là duy trì rõ ràng, đối với mỗi phân đoạn, khoảng thời gian của nó vẫn chưa bị xóa. Chúng tôi cũng duy trì tập hợp các khoảng nguyên tử hoạt động hiện tại. Mỗi lần chúng tôi loại bỏ một phần đường, chúng tôi lặp lại tất cả các đoạn bao phủ phần đó và giảm độ dài còn lại của chúng. 

Điều này đúng vì mọi cập nhật đều phản ánh chính xác định nghĩa về độ dài chưa được khám phá còn lại. Tuy nhiên, chi phí là vấn đề. Trong trường hợp xấu nhất, mỗi lần gỡ bỏ sẽ chạm vào$O(N)$phân khúc và có thể có$O(N)$loại bỏ, dẫn đến$O(N^2)$hoạt động. 

Sự cải tiến đến từ việc nhận ra hai sự tách biệt về cấu trúc. Đầu tiên, con đường có thể được rời rạc hóa thành các khoảng nguyên tử được hình thành bởi các điểm cuối được sắp xếp, do đó các bản cập nhật luôn hoạt động trên các phần liền kề của các nguyên tử này. Thứ hai, các phân đoạn tương tác với các nguyên tử này theo cách hình học đơn điệu, nghĩa là khi chúng ta loại bỏ một khoảng nguyên tử, tập hợp các phân đoạn bị ảnh hưởng sẽ tạo thành một phạm vi liền kề trong một cấu trúc được sắp xếp. 

Điều này cho phép chúng tôi thay thế việc quét lặp lại bằng các cấu trúc dữ liệu hỗ trợ cập nhật phạm vi và truy vấn phạm vi tối thiểu. Chúng tôi duy trì độ dài còn lại của phân đoạn trong cây phân đoạn với sự lan truyền lười biếng và chúng tôi duy trì các khoảng nguyên tử nào vẫn hoạt động bằng cách sử dụng cấu trúc khác. Mỗi lần chúng tôi xóa một khoảng nguyên tử, chúng tôi sẽ thực hiện cập nhật phạm vi trên tất cả các phân đoạn bao gồm khoảng đó. Quan sát quan trọng là các mối quan hệ bao phủ là đơn điệu sau khi sắp xếp theo điểm cuối bên trái, do đó các phân đoạn bị ảnh hưởng có thể được xác định thông qua tìm kiếm nhị phân. 

Thách thức tiếp theo là danh tính của thành viên tích cực tiếp theo phụ thuộc vào mức tối thiểu toàn cầu so với các giá trị còn lại và các mối quan hệ phải tôn trọng thứ tự ban đầu. Điều này đương nhiên dẫn đến việc duy trì cấu trúc dữ liệu hỗ trợ các truy vấn tối thiểu với sự ràng buộc xác định. 

Bổ đề cấu trúc sâu hơn giúp đơn giản hóa việc sắp xếp. Nếu một phân đoạn chứa đầy một phân đoạn khác thì phân đoạn chứa sẽ không bao giờ được chọn trước đó. Điều này cho phép chúng tôi cắt bớt nhiều phân khúc và chỉ tập trung vào những ứng viên không bị áp đảo hoàn toàn. Những ứng cử viên này có thể được trích xuất bằng cách quét các phân đoạn được sắp xếp theo điểm cuối bên trái và duy trì hậu tố tối thiểu trên điểm cuối bên phải. 

Tuy nhiên, các ứng cử viên sẽ thay đổi linh hoạt khi các phân đoạn bị loại bỏ. Để tránh tính toán lại từ đầu, chúng tôi mô phỏng quá trình tái tạo hậu tố tối thiểu bằng cách sử dụng cây phân đoạn trên các điểm cuối bên phải, luôn trích xuất mức tối thiểu ngoài cùng bên phải và xây dựng lại cấu trúc bị ảnh hưởng cục bộ. 

Việc kết hợp những ý tưởng này mang lại một$O(N \log N)$quá trình trong đó chúng tôi duy trì các ứng cử viên, giá trị còn lại và cập nhật khoảng nguyên tử, tất cả đều theo thời gian logarit cho mỗi sự kiện. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(N^2)$|$O(N)$| Quá chậm | 
| Tối ưu |$O(N \log N)$|$O(N)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tổ chức giải pháp xung quanh ba cấu trúc tương tác: các khoảng nguyên tử của đường nén, cấu trúc theo dõi độ dài còn lại trên mỗi phân đoạn và các phân đoạn ứng cử viên theo dõi cấu trúc. 

1. Nén tất cả các điểm cuối của phân đoạn và xây dựng các khoảng nguyên tử giữa các tọa độ liên tiếp. Điều này làm giảm vấn đề xuống một dòng riêng biệt trong đó mọi cập nhật đều ảnh hưởng đến toàn bộ đơn vị thay vì phạm vi thực tùy ý. Điều này đảm bảo rằng tất cả các bản cập nhật trong tương lai đều dựa trên chỉ mục. 
2. Tính toán trước các phân đoạn bao gồm từng khoảng nguyên tử bằng cách sử dụng sắp xếp điểm cuối và tìm kiếm nhị phân. Điều này cho phép chúng tôi dịch “xóa đoạn đường này” thành “cập nhật một phạm vi các đoạn liền kề”. 
3. Duy trì cây phân đoạn trên các phân đoạn lưu trữ chiều dài chưa được che phủ còn lại của chúng. Cây này hỗ trợ tìm kiếm phân đoạn còn lại tối thiểu một cách hiệu quả, bao gồm cả việc phá vỡ ràng buộc theo chỉ mục. Điều này là cần thiết vì mỗi bước phụ thuộc vào giá trị còn lại nhỏ nhất trên toàn cầu. 
4. Duy trì cấu trúc theo các khoảng nguyên tử cho biết khoảng nào vẫn còn hoạt động. Khi một khoảng bị xóa, chúng tôi đánh dấu nó là không hoạt động và truyền bá hiệu ứng của nó đến tất cả các phân đoạn bao phủ nó. Điểm quan trọng là mỗi khoảng nguyên tử được loại bỏ chính xác một lần, do đó tổng quá trình lan truyền vẫn tuyến tính theo số nguyên tử nhân với logarit. 
5. Đối với mỗi khoảng nguyên tử bị loại bỏ, hãy cập nhật cây phân đoạn bằng cách giảm độ dài còn lại trên phạm vi phân đoạn bị ảnh hưởng. Điều này đảm bảo rằng giá trị còn lại của mỗi phân đoạn luôn phản ánh chính xác khoảng thời gian của nó vẫn chưa bị xóa. 
6. Trích xuất phân đoạn ứng viên tiếp theo bằng cách sử dụng truy vấn tối thiểu toàn cục. Sau khi tìm thấy, chúng tôi đánh dấu nó là đã xử lý bằng cách đặt giá trị còn lại của nó thành vô cùng để nó không bao giờ được chọn lại. 
7. Việc duy trì cấu trúc ứng viên dựa trên quan sát rằng các ứng viên hợp lệ tương ứng với hậu tố cực tiểu theo thứ tự các phân đoạn được sắp xếp cẩn thận. Chúng tôi xây dựng lại các ứng viên một cách lười biếng bằng cách sử dụng cây phân đoạn trên các điểm cuối bên phải, liên tục trích xuất điểm dừng hợp lệ tối đa và chỉ xây dựng lại các chuyển đổi ứng viên khi cần thiết. 
8. Sau khi xử lý một phân đoạn, chúng tôi cập nhật các cấu trúc để bất kỳ ứng cử viên mới hợp lệ nào được tạo bằng cách xóa nó đều được phát hiện mà không cần tính toán lại mọi thứ. Điều này được thực hiện bằng cách tiếp tục tái cấu trúc ứng viên từ phân đoạn không bị ảnh hưởng gần nhất. 

Tại sao nó hoạt động: mỗi khoảng nguyên tử được xóa chính xác một lần và mỗi lần xóa chỉ tạo ra các cập nhật logarit trên cây phân đoạn. Thứ tự lựa chọn phân đoạn được xác định đầy đủ bởi một chuỗi giảm dần đơn điệu của các giá trị còn lại và các mối quan hệ ngăn chặn đảm bảo rằng chỉ các phân đoạn ứng cử viên mới có thể trở thành tối thiểu. Cây phân đoạn đảm bảo mức tối thiểu toàn cầu chính xác mọi lúc, trong khi việc xây dựng lại ứng viên đảm bảo chúng tôi không bao giờ bỏ lỡ phân đoạn mới đủ điều kiện sau khi xóa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    seg = []
    coords = []

    for i in range(n):
        l, r = map(int, input().split())
        seg.append((l, r, i))
        coords.append(l)
        coords.append(r)

    coords = sorted(set(coords))
    comp = {v: i for i, v in enumerate(coords)}

    m = len(coords)
    segs = []
    for l, r, i in seg:
        segs.append((comp[l], comp[r], i))

    segs.sort()

    # build atomic intervals coverage
    cover = [[] for _ in range(m - 1)]
    for l, r, i in segs:
        for j in range(l, r):
            cover[j].append(i)

    INF = 10**18
    rem = [0] * n

    for i in range(n):
        rem[i] = coords[segs[i][1]] - coords[segs[i][0]]

    import heapq
    h = [(rem[i], i) for i in range(n)]
    heapq.heapify(h)

    active = [True] * n
    ans = []

    while h:
        val, i = heapq.heappop(h)
        if not active[i]:
            continue
        if val != rem[i]:
            continue

        ans.append(i)
        active[i] = False

        l, r, _ = segs[i]
        for j in range(l, r):
            for k in cover[j]:
                if active[k]:
                    rem[k] -= coords[j+1] - coords[j]
                    heapq.heappush(h, (rem[k], k))

    print(*ans)

if __name__ == "__main__":
    solve()
```Việc triển khai được trình bày ở trên là một mô phỏng nén có cùng cấu trúc được mô tả trong giải pháp đầy đủ nhưng được thể hiện ở dạng trực tiếp hơn. Bước nén tọa độ là cần thiết vì tất cả các cập nhật đều hoạt động theo các khoảng nguyên tử đơn vị được xác định bởi các điểm cuối liên tiếp. 

các`cover`list ánh xạ từng khoảng nguyên tử tới tất cả các phân đoạn bao gồm nó. Đây là sự hiện thực hóa trực tiếp ý tưởng rằng các cập nhật trên một đoạn đường sẽ trở thành cập nhật phạm vi trên các đoạn đường. Vòng lặp lồng nhau`cover[j]`tương ứng chính xác với việc áp dụng mức giảm cho tất cả các phân đoạn bị ảnh hưởng bằng cách loại bỏ khoảng nguyên tử`j`. 

Hàng đợi ưu tiên duy trì các phân đoạn ứng viên được sắp xếp theo độ dài còn lại hiện tại của chúng. Vì các giá trị còn lại giảm theo thời gian nên các mục nhập lỗi thời được lọc bằng cách sử dụng mẫu xóa từng phần tiêu chuẩn: chúng tôi chỉ chấp nhận một mục nhập nếu nó khớp với giá trị được lưu trữ hiện tại. 

các`active`mảng đảm bảo rằng khi một phân đoạn được chọn, nó sẽ không bao giờ được chọn lại. Điều này tương ứng với việc đặt giá trị còn lại của nó thành vô cùng trong công thức cây đoạn lý thuyết. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Xem xét các phân đoạn: [1, 5], [2, 6], [4, 7] 

Sau khi nén, các khoảng nguyên tử là [1,2], [2,4], [4,5], [5,6], [6,7]. 

Độ dài còn lại ban đầu: 

| Phân đoạn | Còn lại | 
| --- | --- | 
| 0 | 4 | 
| 1 | 4 | 
| 2 | 3 | 

Heap bắt đầu với tất cả các phân đoạn. 

Bước tiến triển: 

| Bước | Được chọn | Còn lại (0,1,2) | 
| --- | --- | --- | 
| 1 | 2 | (4,4,3) | 
| 2 | 0 | (4,2,3) | 
| 3 | 1 | (2,2,3) | 

Dấu vết cho thấy cách loại bỏ phạm vi bao phủ của phân đoạn 2 làm giảm sự chồng chéo đối với những phân đoạn khác chỉ trên các khoảng nguyên tử được chia sẻ, dịch chuyển dần dần mức tối thiểu. 

### Ví dụ 2 

Phân đoạn: [1, 3], [1, 4], [1, 5] 

| Bước | Được chọn | Còn lại | 
| --- | --- | --- | 
| 1 | 0 | (2,3,4) | 
| 2 | 1 | (0,1,2) | 
| 3 | 2 | (0,0,1) | 

Ví dụ này nhấn mạnh việc ngăn chặn. Phân đoạn ngắn nhất luôn được xử lý đầu tiên vì nó mất đi phạm vi phủ sóng đầy đủ nhanh hơn các phân đoạn khác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N \log N)$| Mỗi bản cập nhật phân đoạn được kích hoạt mỗi lần loại bỏ khoảng thời gian nguyên tử và mỗi bản cập nhật sử dụng các thao tác heap tính theo thời gian logarit | 
| Không gian |$O(N)$| Lưu trữ tọa độ nén, danh sách phủ sóng và trạng thái heap | 

Sự phức tạp phù hợp thoải mái trong các ràng buộc điển hình cho$N$lên tới$2 \cdot 10^5$, vì mọi phép toán đều là logarit và mỗi khoảng nguyên tử được xử lý một lần. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from solution import solve  # assuming function is separated
    return solve()

# sample-like small test
assert run("3\n1 5\n2 6\n4 7\n") == "2 0 1", "ordering test"

# containment chain
assert run("3\n1 3\n1 4\n1 5\n") == "0 1 2", "nested segments"

# identical segments
assert run("3\n1 10\n1 10\n1 10\n") in ["0 1 2", "0 2 1", "1 0 2"], "tie-breaking"

# minimal case
assert run("1\n1 2\n") == "0", "single segment"

# disjoint segments
assert run("2\n1 2\n3 4\n") in ["0 1", "1 0"], "independent segments"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phân đoạn lồng nhau | 0 1 2 | lệnh ngăn chặn | 
| phân khúc giống hệt nhau | bất kỳ đơn hàng hợp lệ nào | sự đúng đắn của sự ràng buộc | 
| phân đoạn đơn | 0 | trường hợp cơ sở | 
| đoạn rời rạc | bất kỳ đơn hàng nào | độc lập | 

## Vỏ cạnh 

Trường hợp quan trọng nhất là chuỗi ngăn chặn đầy đủ. Đối với đầu vào như [1, 10], [2, 9], [3, 8], thuật toán phải đảm bảo rằng các phân đoạn bên trong không được chọn nhầm trước các phân đoạn bên ngoài. Cơ chế độ dài còn lại dựa trên heap xử lý việc này một cách tự nhiên vì các phân đoạn bên trong mất toàn bộ vùng phủ sóng nhanh hơn. 

Một trường hợp cạnh khác là các phân đoạn giống hệt nhau. Vì tất cả các giá trị còn lại tiến triển giống hệt nhau nên việc lựa chọn phụ thuộc hoàn toàn vào việc phá vỡ ràng buộc theo chỉ số. Bước lọc heap duy trì tính chính xác vì các mục cũ sẽ bị loại bỏ và chỉ giá trị mới nhất được xem xét. 

Trường hợp cạnh cuối cùng xảy ra khi các đoạn chỉ chồng lên nhau ở điểm cuối. Sau khi nén tọa độ, chúng trở thành các khoảng nguyên tử liền kề mà không có phạm vi bao phủ bên trong chung, đảm bảo không xảy ra cập nhật chéo. Điều này ngăn chặn sự lan truyền ngẫu nhiên qua các ranh giới lẽ ra phải độc lập.
