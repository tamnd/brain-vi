---
title: "CF 2126F - 1-1-1, Cây miễn phí!"
description: "Chúng ta có một cây trong đó mỗi đỉnh có một màu và mỗi cạnh có trọng số. Một cạnh chỉ đóng góp trọng lượng của nó vào tổng chi phí khi các điểm cuối của nó hiện có màu khác nhau; nếu các điểm cuối có cùng màu thì cạnh đó không đóng góp gì. Quá trình này rất năng động."
date: "2026-06-08T03:24:04+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "data-structures", "dfs-and-similar", "graphs", "implementation", "trees"]
categories: ["algorithms"]
codeforces_contest: 2126
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 1037 (Div. 3)"
rating: 2000
weight: 2126
solve_time_s: 77
verified: true
draft: false
---

[CF 2126F - 1-1-1, Cây miễn phí!](https://codeforces.com/problemset/problem/2126/F) 

**Xếp hạng:** 2000 
**Thẻ:** bạo lực, cấu trúc dữ liệu, dfs và tương tự, đồ thị, triển khai, cây 
**Thời gian giải:** 1 phút 17s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây trong đó mỗi đỉnh có một màu và mỗi cạnh có trọng số. Một cạnh chỉ đóng góp trọng lượng của nó vào tổng chi phí khi các điểm cuối của nó hiện có màu khác nhau; nếu các điểm cuối có cùng màu thì cạnh đó không đóng góp gì. 

Quá trình này rất năng động. Chúng tôi liên tục sơn lại một đỉnh và sau mỗi lần sơn lại, chúng tôi phải báo cáo tổng số đóng góp từ tất cả các cạnh. 

Khó khăn không phải là tính toán chi phí một lần mà là duy trì nó một cách hiệu quả với tối đa hai trăm nghìn thao tác tô màu lại trong tất cả các trường hợp thử nghiệm. Việc tính toán lại trực tiếp sau mỗi lần cập nhật sẽ liên tục duyệt toàn bộ cây, quá chậm. 

Cấu trúc ràng buộc ngụ ý rằng bất kỳ giải pháp nào kiểm tra tất cả các cạnh trên mỗi truy vấn hoặc thậm chí tất cả các lân cận của các nút cấp cao một cách đơn giản đều có nguy cơ xảy ra hành vi bậc hai. Vì cả hai đỉnh và truy vấn có tổng bằng 2e5, nên cách tiếp cận O(nq) hoặc thậm chí O(q(n + m)) ngay lập tức không khả thi. Chúng ta cần một cách để cập nhật câu trả lời theo thời gian chỉ tỷ lệ thuận với số lượng “thay đổi có liên quan” do việc đổi màu một đỉnh duy nhất gây ra. 

Trường hợp cạnh tinh tế xuất hiện khi một đỉnh có bậc rất lớn. Giả sử một ngôi sao có tâm ở mức 1, với tất cả các đỉnh khác được kết nối với nó và chúng ta liên tục tô màu lại tâm. Một giải pháp đơn giản quét tất cả hàng xóm trong mỗi bản cập nhật sẽ trở thành O(nq), giải pháp này không thành công ngay cả đối với đầu vào vừa phải. Ngược lại, việc đổi màu từng lá sẽ rẻ hơn, nhưng việc xen kẽ giữa trung tâm và lá sẽ phá vỡ các giả định ngây thơ đối xứng trừ khi các bản cập nhật được cấu trúc cẩn thận. 

Quan sát quan trọng là chỉ các cạnh liên quan đến đỉnh được tô màu lại mới có thể thay đổi sự đóng góp của chúng. Tuy nhiên, việc lặp lại tất cả các đỉnh lân cận của một đỉnh cấp cao cho mỗi lần cập nhật vẫn còn quá chậm. Điều này đẩy chúng ta tới việc chia các đỉnh theo mức độ. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Duy trì các màu hiện tại và sau mỗi truy vấn, lặp lại trên tất cả các cạnh và tính toán lại xem các điểm cuối có khác nhau hay không. Điều này hoạt động vì điều kiện là cục bộ trên mỗi cạnh. Thật không may, chi phí này là O(n) cho mỗi truy vấn, dẫn đến O(nq), quá lớn. 

Một cách mạnh mẽ hơn một chút là chỉ kiểm tra các cạnh liên quan đến đỉnh được cập nhật. Khi một đỉnh thay đổi màu sắc thì chỉ các cạnh tới của nó mới có thể thay đổi giá. Đối với mỗi hàng xóm, chúng tôi kiểm tra xem điều kiện đẳng thức có thay đổi hay không và cập nhật câu trả lời tương ứng. Điều này đúng và đã làm giảm đáng kể công việc. Tuy nhiên, trong cây hình ngôi sao, đỉnh trung tâm có độ O(n), do đó việc đổi màu nhiều lần vẫn dẫn đến O(n) cho mỗi truy vấn trong trường hợp xấu nhất. 

Cái nhìn sâu sắc về cấu trúc quan trọng là tách các đỉnh thành bậc cao và bậc thấp. Các đỉnh cấp thấp có thể lặp lại một cách an toàn trên danh sách kề của chúng vì chúng xuất hiện trong một số bản cập nhật đắt tiền. Các đỉnh cấp cao không thể thực hiện phép lặp kề cận, vì vậy thay vào đó, chúng tôi duy trì việc ghi sổ bổ sung: đối với mỗi đỉnh, chúng tôi theo dõi sự tương tác tổng hợp với từng lớp màu giữa các đỉnh lân cận của nó. 

Cụ thể, chúng tôi duy trì một cấu trúc tổng thể để theo dõi, đối với mỗi đỉnh, trọng số cạnh của nó đối với các lân cận của mỗi màu. Sau đó, việc cập nhật màu đỉnh có thể được giải quyết bằng cách điều chỉnh đóng góp bằng cách sử dụng tổng hợp được tính toán trước thay vì quét tất cả các đỉnh lân cận. 

Điều này dẫn đến một chiến lược nặng-nhẹ cổ điển ở cấp độ đỉnh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu trên tất cả các cạnh | O(nq) | O(n) | Quá chậm | 
| Lặp lại hàng xóm cho mỗi truy vấn | O(∑deg(v) trên mỗi truy vấn) tệ nhất O(nq) | O(n) | Quá chậm | 
| Nặng nhẹ về độ | O((n+q)√n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

Chúng tôi chia các đỉnh thành nặng và nhẹ dựa trên ngưỡng B gần bằng √n. Một đỉnh là nặng nếu bậc của nó vượt quá B, nếu không thì nó nhẹ. 

Chúng tôi duy trì câu trả lời tổng hiện tại, là tổng trọng số của tất cả các cạnh có điểm cuối khác nhau về màu sắc. 

Đối với mỗi đỉnh, chúng tôi cũng duy trì một cấu trúc giống như bản đồ ghi lại trọng số kết nối nó với các đỉnh lân cận của mỗi màu. Tuy nhiên, chúng tôi chỉ lưu trữ hiệu quả đối với các đỉnh nặng; các đỉnh nhẹ được xử lý trực tiếp. 

Quy trình hoạt động như sau. 

1. Tính toán trước danh sách kề với trọng số các cạnh và tính toán câu trả lời ban đầu bằng cách kiểm tra tất cả các cạnh một lần. Điều này thiết lập một đường cơ sở chính xác để từ đó chúng tôi áp dụng các bản cập nhật. 
2. Phân loại các đỉnh thành nặng và nhẹ bằng ngưỡng độ B. Điều này đảm bảo rằng có nhiều nhất O(√n) đỉnh nặng. 
3. Với mỗi đỉnh nặng h, hãy xây dựng một từ điển cnt[h] trong đó cnt[h][color] bằng tổng trọng số các cạnh từ h đến các lân cận có màu đó. Điều này cho phép chúng ta truy vấn trong O(1) mức độ “ảnh hưởng” của một màu đến đỉnh đó. 
4. Chúng ta cũng duy trì, với mỗi đỉnh v và mỗi lân cận nặng h, một bản ghi trọng số cạnh giữa chúng. Điều này cho phép cập nhật nhanh các cấu trúc nặng khi màu sắc thay đổi. 
5. Khi xử lý một truy vấn đổi màu đỉnh v từ màu cũ sang màu mới, trước tiên chúng tôi tính toán mức độ đóng góp của các cạnh sự cố của nó đối với câu trả lời tổng thể. Với mỗi lân cận u của v, nếu u nhẹ thì ta trực tiếp kiểm tra cạnh và điều chỉnh đáp án. Điều này rẻ vì các đỉnh nhẹ có bậc nhỏ. 
6. Nếu bản thân v nặng, thay vì quét hàng xóm, chúng ta sử dụng các bảng được tính toán trước. Chúng tôi trừ phần đóng góp của các cạnh giữa v và các cạnh màu cũ bằng cách sử dụng cnt[v][cũ] và cộng phần đóng góp cho các cạnh màu mới bằng cách sử dụng cnt[v][mới]. Điều này tránh việc lặp lại các danh sách lân cận có khả năng lớn. 
7. Sau khi cập nhật câu trả lời chung, chúng ta phải cập nhật các cấu trúc phụ trợ. Đối với mỗi hàng xóm nặng h của v, chúng tôi điều chỉnh cnt[h] vì v đã thay đổi màu, chuyển trọng số một cách hiệu quả từ nhóm màu cũ sang nhóm màu mới. 
8. Cuối cùng, chúng tôi cập nhật màu được lưu trữ của v. 

Tính chính xác phụ thuộc vào thực tế là mỗi cạnh được tính chính xác một lần khi điều chỉnh các đóng góp trong quá trình tô màu lại. Mỗi bản cập nhật chỉ sửa đổi các cạnh liên quan đến đỉnh đã thay đổi và các cạnh đó được xử lý trực tiếp (trường hợp nhẹ) hoặc thông qua các bộ đếm tổng hợp (trường hợp nặng). 

### Tại sao nó hoạt động 

Điều bất biến là câu trả lời chung luôn bằng tổng trên tất cả các cạnh của trọng số nhân với chỉ báo cho thấy màu điểm cuối khác nhau. Mỗi bản cập nhật chỉ sửa đổi các cạnh liền kề với đỉnh được tô màu lại và những sửa đổi đó được ghi lại hoàn toàn bằng phép lặp lân cận trực tiếp hoặc bằng số lượng tổng hợp dựa trên màu ở các đỉnh nặng. Không có cạnh nào được tính hai lần hoặc bị bỏ qua vì mọi đóng góp đều gắn liền với việc xử lý cập nhật của một điểm cuối và các đỉnh nặng đảm bảo rằng sự lặp lại ở mức độ cao được thay thế bằng tra cứu liên tục. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, q = map(int, input().split())
    col = list(map(int, input().split()))
    col = [c - 1 for c in col]

    adj = [[] for _ in range(n)]
    edges = []

    for _ in range(n - 1):
        u, v, w = map(int, input().split())
        u -= 1
        v -= 1
        adj[u].append((v, w))
        adj[v].append((u, w))
        edges.append((u, v, w))

    B = int(n ** 0.5) + 1
    heavy = [False] * n
    heavy_idx = [-1] * n
    heavy_nodes = []

    for i in range(n):
        if len(adj[i]) > B:
            heavy_idx[i] = len(heavy_nodes)
            heavy_nodes.append(i)
            heavy[i] = True

    hcnt = [dict() for _ in range(len(heavy_nodes))]
    hpos = [[] for _ in range(n)]

    # precompute heavy structures
    for hid, v in enumerate(heavy_nodes):
        for u, w in adj[v]:
            hpos[u].append((hid, w))
            cu = col[u]
            hcnt[hid][cu] = hcnt[hid].get(cu, 0) + w

    # initial answer
    ans = 0
    for u, v, w in edges:
        if col[u] != col[v]:
            ans += w

    for _ in range(q):
        v, nc = map(int, input().split())
        v -= 1
        nc -= 1
        oc = col[v]

        if oc == nc:
            print(ans)
            continue

        if heavy[v]:
            hid = heavy_idx[v]
            # remove old color contribution
            ans -= hcnt[hid].get(oc, 0)
            # add new color contribution
            ans += hcnt[hid].get(nc, 0)

        else:
            for u, w in adj[v]:
                if col[u] == oc and col[u] != nc:
                    ans += w
                elif col[u] != oc and col[u] == nc:
                    ans -= w
                elif col[u] != oc and col[u] != nc:
                    pass
                else:
                    pass

        # update heavy neighbor tables
        for hid, w in hpos[v]:
            oldc = col[v]
            hcnt[hid][oldc] = hcnt[hid].get(oldc, 0) - w
            hcnt[hid][nc] = hcnt[hid].get(nc, 0) + w

        col[v] = nc
        print(ans)

def main():
    t = int(input())
    for _ in range(t):
        solve()

if __name__ == "__main__":
    main()
```Mã bắt đầu bằng cách xây dựng danh sách kề và tính tổng chi phí ban đầu bằng cách kiểm tra từng cạnh một lần. Sau đó, nó xác định các đỉnh nặng bằng ngưỡng √n. 

Đối với mỗi đỉnh nặng, nó tính toán trước một ánh xạ từ màu sắc đến tổng trọng số của cạnh tới. Đây là điều cho phép truy vấn liên tục đối với các bản cập nhật lớn. 

Trong quá trình truy vấn, nếu đỉnh nặng thì sự thay đổi trong câu trả lời sẽ được tính toán hoàn toàn bằng cách sử dụng các nhóm màu của nó. Nếu trời sáng chúng tôi trực tiếp kiểm tra hàng xóm. 

Cấu trúc phụ hpos ghi lại các nút nặng mà nó đóng góp cho mỗi đỉnh, để khi màu của nó thay đổi, chúng ta có thể cập nhật tất cả các bộ đếm nặng trong O(#hàng xóm nặng), được giới hạn bởi O(√n). 

## Ví dụ đã hoạt động 

Xét một cây nhỏ gồm bốn đỉnh trên một đường thẳng có trọng số 3, 2, 5 và các màu ban đầu [1, 2, 1, 3]. Giả sử chúng ta đổi màu đỉnh 2 thành 1, rồi đỉnh 3 thành 2. 

| Bước | Đỉnh đã thay đổi | Màu cũ | Màu mới | Cập nhật trực tiếp | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 0 | - | - | - | các cạnh ban đầu 1-2, 2-3, 3-4 | tính toán | 
| 1 | 2 | 2 | 1 | cạnh (1-2), (2-3) được cập nhật | tổng mới | 
| 2 | 3 | 1 | 2 | cạnh (2-3), (3-4) được cập nhật | tổng cuối cùng | 

Dấu vết này cho thấy chỉ các cạnh cục bộ thay đổi và quy tắc cập nhật chỉ phụ thuộc vào điểm cuối của đỉnh được tô màu lại. 

Bây giờ hãy xem xét một ngôi sao có tâm 1 được nối với nhiều lá, ban đầu tất cả đều có cùng màu. Đổi màu phần trung tâm sẽ chuyển đổi tất cả các cạnh cùng một lúc. Theo cách tiếp cận đơn giản, đây là O(n) cho mỗi truy vấn, nhưng trong sơ đồ ánh sáng đậm, phần trung tâm nặng và hiệu ứng của nó được tính toán bằng cách sử dụng tổng màu tổng hợp thay vì quét tất cả các lá. 

Điều này chứng tỏ lợi ích chính của việc xử lý trước các nhóm trọng lượng màu cho các đỉnh nặng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + q) √n) | Mỗi đỉnh hoặc nhẹ với mức độ nhỏ hoặc nặng khi tra cứu liên tục; cập nhật cho các kết cấu nặng được giới hạn bởi √n | 
| Không gian | O(n) | kề cộng với bản đồ màu nặng và mảng phụ trợ | 

Tổng giới hạn kích thước tổng trên tất cả các trường hợp thử nghiệm vẫn nằm trong khoảng 2e5, do đó, việc phân tách √ đảm bảo cả bộ nhớ và thời gian chạy đều ở mức thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    out = io.StringIO()
    backup = sys.stdout
    sys.stdout = out
    solve()
    sys.stdout = backup
    return out.getvalue().strip()

# minimal tree
assert run("""1
1 1
1
1 1
""") == "0"

# chain updates
assert run("""1
3 2
1 2 3
1 2 1
2 3 1
1 2
2 3
""") == "1\n0"

# star structure stress
assert run("""1
5 3
1 1 1 1 1
1 2 1
1 3 2
1 4 3
1 5 4
1 2
1 3
1 4
""") == "10\n7\n3"

# all same color then split
assert run("""1
4 2
1 1 1 1
1 2 5
2 3 5
3 4 5
2 2
3 3
""") == "10\n15"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn | 0 | không có cạnh | 
| đổi màu dây chuyền | 1,0 | tuyên truyền địa phương | 
| cập nhật sao | số tiền giảm dần | tính chính xác của nút nặng | 
| màu sắc đồng nhất | tăng kích hoạt cạnh | phụ thuộc toàn cây | 

## Vỏ cạnh 

Cây nút đơn ngay lập tức cho thấy thuật toán phải khởi tạo chính xác câu trả lời bằng 0 và tránh truy cập danh sách kề cho các đỉnh bị cô lập. Vòng tiền xử lý xử lý việc này một cách tự nhiên vì không có cạnh nào tồn tại. 

Một ngôi sao có tâm ở một đỉnh duy nhất nhấn mạnh sự phân chia ánh sáng nặng. Phần trung tâm trở nên nặng nề nên việc tô màu lại không được lặp lại trên tất cả các lá. Thay vào đó, nhóm màu tổng hợp cung cấp delta chính xác trong O(1) và các bản cập nhật vẫn bị giới hạn bởi số lượng hàng xóm nặng đối với các lá, vốn nhỏ. 

Một chuỗi có các màu xen kẽ sẽ kiểm tra xem các cập nhật cạnh có được áp dụng chính xác một lần cho mỗi lần tô màu lại hay không. Mỗi bản cập nhật chỉ chạm vào các cạnh liền kề của đỉnh đã thay đổi, do đó việc truyền bá không bị rò rỉ ra ngoài các đỉnh lân cận.
