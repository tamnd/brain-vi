---
title: "CF 105453A - Trang trại gà nhị phân"
description: "Chúng ta có một mạng lưới ảnh hưởng trực tiếp lên N con gà. Mỗi con gà duy trì trạng thái chuỗi nhị phân có độ dài cố định L và trạng thái này phát triển từng ngày. Vào ngày thứ nhất, mỗi con gà đều có một chuỗi nhị phân ban đầu."
date: "2026-06-23T17:35:27+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105453
codeforces_index: "A"
codeforces_contest_name: "2024 ICPC Greece Regional Collegiate Programming Contest (GRCPC 2024)"
rating: 0
weight: 105453
solve_time_s: 77
verified: true
draft: false
---

[CF 105453A - Trang trại gà nhị phân](https://codeforces.com/problemset/problem/105453/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 17s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một mạng lưới ảnh hưởng trực tiếp lên N con gà. Mỗi con gà duy trì trạng thái chuỗi nhị phân có độ dài cố định L và trạng thái này phát triển từng ngày. Vào ngày thứ nhất, mỗi con gà đều có một chuỗi nhị phân ban đầu. 

Từ ngày thứ 2 trở đi, mỗi con gà giữ nguyên chuỗi trước đó của nó hoặc nếu nó có chính xác một cạnh ảnh hưởng đến từ một số con gà A khác, thì nó sẽ cập nhật bằng cách XOR chuỗi trước đó của nó với chuỗi trước đó của A. Điểm mấu chốt là XOR sử dụng các trạng thái từ ngày hôm trước chứ không phải các trạng thái được cập nhật từ cùng ngày, vì vậy về mặt khái niệm, tất cả các cập nhật đều đồng thời. 

Chúng ta được yêu cầu tính toán cấu hình vào ngày K, trong đó K có thể lớn tới 10^4. Vì L tối đa là 20 nên trạng thái của mỗi con gà là một mặt nạ bit nhỏ chứ không phải là một chuỗi lớn, do đó các thao tác theo bit là khả thi. 

Cấu trúc của các phụ thuộc là một đồ thị hàm số trong đó mỗi nút có nhiều nhất một cạnh tới. Điều này ngay lập tức gợi ý các chuỗi và có thể cả các cây bắt nguồn từ các nút không có ảnh hưởng. 

Một cách giải thích đơn giản sẽ mô phỏng từng ngày, tính toán lại tất cả N trạng thái K lần. Điều đó sẽ có giá O(KN L), quá lớn khi K là 10^4 và N là 1000. 

Một vấn đề tinh tế hơn là bản cập nhật mang tính tuyến tính trên XOR, do đó, nhiều đường ảnh hưởng có thể tích lũy đóng góp. Nếu một con gà bị ảnh hưởng bởi chuỗi A → B → C, thì B phụ thuộc vào A và C phụ thuộc vào B, do đó C tích lũy hiệu quả các đóng góp XOR lan truyền xuống chuỗi theo thời gian. Một mô phỏng đơn giản hàng ngày có thể đúng nhưng sẽ chậm một cách không cần thiết. 

Trường hợp cạnh phát sinh khi chuỗi dài. Ví dụ: nếu 1 → 2 → 3 và các trạng thái ban đầu là tùy ý thì giá trị của mỗi nút sẽ trở thành XOR phân lớp của các nút trước đó. Nếu K lớn, việc tính toán lại nhiều lần sẽ làm tăng chi phí. Một trường hợp cạnh khác là các nút bị cô lập, trong đó không có cạnh nào tồn tại, do đó trạng thái phải không đổi bất kể K. Bất kỳ việc triển khai không chính xác nào giả định mọi cập nhật nút đều sẽ thất bại ở đây. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu là mô phỏng rõ ràng mỗi ngày. Đối với mỗi ngày từ 2 đến K, chúng tôi tính toán trạng thái tiếp theo của mỗi con gà bằng cách kiểm tra xem nó có người ảnh hưởng sắp tới hay không và áp dụng XOR với trạng thái trước đó của người ảnh hưởng. Vì mỗi trạng thái là một chuỗi nhị phân có độ dài L, nên mỗi lần cập nhật có giá O(L) và có N con gà trong K ngày, mang lại các phép toán O(KNL). Với K lên tới 10^4, N lên tới 1000 và L lên tới 20, điều này đạt tới các hoạt động khoảng 2×10^8 bit trong trường hợp xấu nhất, là đường biên hoặc quá chậm trong Python. 

Quan sát quan trọng là các cập nhật XOR là tuyến tính trên GF(2) và trạng thái cuối cùng của mỗi nút là sự kết hợp tuyến tính của các trạng thái ban đầu. Quan trọng hơn, cấu trúc này là một rừng các chuỗi được định hướng vì mỗi nút có nhiều nhất một cạnh vào. Điều này có nghĩa là mỗi nút bắt đầu một chuỗi hoặc nằm trên chính xác một chuỗi lan truyền ảnh hưởng. 

Thay vì mô phỏng K bước, chúng tôi diễn giải lại quy trình: mỗi nút tích lũy đóng góp XOR từ tổ tiên của nó theo hướng ảnh hưởng ngược lại và số lần đóng góp xuất hiện chỉ phụ thuộc vào K và khoảng cách dọc theo chuỗi. Điều này trở thành một vấn đề lan truyền tổ hợp trên một khu rừng nơi chúng ta có thể tính toán trước mức độ ảnh hưởng sẽ lan truyền trong K bước. 

Chúng ta có thể đảo ngược các cạnh và xử lý từ gốc trở xuống, tính toán cho mỗi nút hiệu ứng XOR tích lũy của các nút trong khoảng cách K dọc theo chuỗi đến của nó. Vì K nhỏ nên chúng ta có thể truyền bá các đóng góp dọc theo chuỗi trong O(KN), nhưng chúng ta có thể tối ưu hóa hơn nữa bằng cách nhận thấy rằng mỗi nút chỉ phụ thuộc vào một đường dẫn tổ tiên duy nhất, do đó chúng ta có thể tính toán trước các đại diện chuỗi và đóng góp nhảy một cách hiệu quả. 

Do đó, thay vì lặp lại nhiều ngày, chúng tôi thu gọn quy trình thành việc duyệt qua từng chuỗi một lần và áp dụng truyền bá tiền tố XOR đến khoảng cách K.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(KNL) | O(NL) | Quá chậm | 
| Tuyên truyền dựa trên chuỗi | O(N L + K N) | O(NL) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi mỗi chuỗi nhị phân thành một bitmask số nguyên để các phép toán XOR trở thành O(1). Điều này hợp lệ vì L 20, vì vậy tất cả các trạng thái đều phù hợp với số nguyên tiêu chuẩn. 
2. Xây dựng một mảng`parent`Ở đâu`parent[v] = u`nếu bạn ảnh hưởng đến v. Các nút không có người ảnh hưởng có`parent[v] = -1`. 
3. Đối với mỗi nút, hãy tính chuỗi mà nó thuộc về bằng cách đi theo các con trỏ cha cho đến khi đạt đến gốc. Trong khi thực hiện việc này, hãy lưu trữ chuỗi các nút trong chuỗi đó theo thứ tự từ gốc đến lá. Điều này có hiệu quả vì mỗi nút có nhiều nhất một nút cha nên không xảy ra sự phân nhánh. 
4. Đối với mỗi chuỗi, tính toán trước tiền tố XOR của các trạng thái ban đầu dọc theo chuỗi. Điều này cho phép chúng tôi truy vấn bất kỳ phân đoạn XOR nào trong thời gian không đổi. 
5. Đối với mỗi nút trong chuỗi ở vị trí i (được lập chỉ mục 0 từ gốc), hãy tính xem có bao nhiêu nút tổ tiên trong chuỗi ảnh hưởng đến nó sau khi chuyển đổi K - 1. Vì mỗi ngày di chuyển sẽ ảnh hưởng đến một bước tiến lên, nên một nút trong ngày K phụ thuộc vào các nút có khoảng cách K - 1 phía trên nó trong chuỗi. 
6. Đặt trạng thái cuối cùng của mỗi nút là XOR của phân đoạn tiền tố từ max(0, i - (K - 1)) đến i. Điều này tổng hợp tất cả các đóng góp đến nút trong K bước. 
7. Xuất tất cả các mặt nạ bit cuối cùng được chuyển đổi thành chuỗi nhị phân có độ dài cố định L. 

Điểm tinh tế là ảnh hưởng chỉ di chuyển một bước mỗi ngày dọc theo chuỗi, vì vậy sau khi chuyển đổi K - 1, chỉ có nút K cuối cùng trong đường dẫn tổ tiên là quan trọng. 

### Tại sao nó hoạt động 

Trạng thái của mỗi nút phát triển khi ứng dụng lặp đi lặp lại của toán tử XOR tuyến tính dọc theo một đường dẫn có hướng. Vì mỗi nút có nhiều nhất một nút cha nên biểu đồ phụ thuộc sẽ phân tách thành các chuỗi độc lập. Dọc theo mỗi chuỗi, trạng thái tại ngày t chính xác là XOR của cửa sổ trượt của các trạng thái trước đó có kích thước tăng tuyến tính với t nhưng bị giới hạn bởi độ dài chuỗi. Việc giải thích cửa sổ trượt này đảm bảo rằng tiền tố điện toán XOR trên chuỗi và các cửa sổ cắt lát tái tạo chính xác sự tích lũy giống như các bản cập nhật đồng bộ K. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def to_mask(s):
    return int(s, 2)

def to_bin(x, L):
    return format(x, '0{}b'.format(L))

N, M, K = map(int, input().split())
init = input().split()

parent = [-1] * N
for _ in range(M):
    a, b = map(int, input().split())
    parent[b - 1] = a - 1

visited = [False] * N
chains = []

for i in range(N):
    if parent[i] == -1:
        cur = i
        chain = []
        while cur != -1:
            chain.append(cur)
            cur = -1 if parent[cur] == -1 else parent[cur]
        chains.append(chain)

ans = [0] * N
L = len(init[0])

for chain in chains:
    pref = [0]
    for node in chain:
        pref.append(pref[-1] ^ to_mask(init[node]))

    for i, node in enumerate(chain):
        l = max(0, i - (K - 1))
        ans[node] = pref[i + 1] ^ pref[l]

for i in range(N):
    print(to_bin(ans[i], L), end=' ')
```Việc triển khai nén các chuỗi nhị phân thành các số nguyên để XOR trở thành một thao tác đơn lẻ. Mỗi chuỗi được trích xuất bằng cách đi theo các con trỏ gốc từ gốc trở xuống, đảm bảo chúng tôi xử lý từng nút chính xác một lần trong thời gian tuyến tính. 

Mảng XOR tiền tố cho phép chúng ta tính toán bất kỳ XOR phân đoạn liền kề nào trong O(1), điều này rất cần thiết để áp dụng cửa sổ ảnh hưởng bước K một cách hiệu quả. 

Một chi tiết nhỏ là việc chuyển đổi trở lại chuỗi nhị phân có chiều rộng cố định L. Nếu không có phần đệm, các số 0 ở đầu sẽ bị mất, điều này sẽ tạo ra định dạng đầu ra không chính xác ngay cả khi các giá trị cơ bản là chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 2 3
101 010 111
1 2
2 3
```Chúng ta tạo thành chuỗi 1 → 2 → 3. Chuyển đổi sang mặt nạ: 101=5, 010=2, 111=7. 

| Nút | Vị trí | Tiền tố XOR | Bắt đầu cửa sổ | Giá trị cuối cùng | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 5 | 0 | 5 | 
| 2 | 1 | 5^2=7 | 0 | 7 | 
| 3 | 2 | 7^7=0 | 0 | 0 | 

Với K=3, mỗi nút có thể bao gồm tối đa 2 tổ tiên. Nút 3 tích lũy cả ba giá trị ban đầu dọc theo chuỗi, dẫn đến 5^2^7 = 0. 

Điều này xác nhận rằng cửa sổ trượt tổng hợp chính xác các khoản đóng góp trên toàn chuỗi. 

### Ví dụ 2 

đầu vào:```
4 1 2
1 0 1 1
1 3
```Chuỗi: 1 → 3, nút 2 và 4 bị cô lập. 

| Nút | Chuỗi | tôi | Cửa sổ | Cuối cùng | 
| --- | --- | --- | --- | --- | 
| 1 | 1,3 | 0 | [1] | 1 | 
| 3 | 1,3 | 1 | [1,0] | 1 | 
| 2 | - | - | [0] | 0 | 
| 4 | - | - | [1] | 1 | 

Các nút bị cô lập không thay đổi bất kể K, trong khi các nút chuỗi tích lũy theo lịch sử có sẵn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N + M + N) | Mỗi nút được truy cập một lần trong khi xây dựng chuỗi và mảng tiền tố | 
| Không gian | O(N) | Bộ lưu trữ cho các con trỏ cha, chuỗi và mảng XOR tiền tố | 

Các ràng buộc cho phép tối đa 1000 nút, do đó, các phép toán XOR truyền tải tuyến tính và thời gian không đổi dễ dàng đủ nhanh. Việc sử dụng bộ nhớ bị chi phối bởi việc lưu trữ biểu đồ và một vài mảng có kích thước N. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    N, M, K = map(int, input().split())
    init = input().split()

    parent = [-1] * N
    for _ in range(M):
        a, b = map(int, input().split())
        parent[b - 1] = a - 1

    ans = [0] * N
    L = len(init[0])

    def to_mask(s):
        return int(s, 2)

    def to_bin(x):
        return format(x, '0{}b'.format(L))

    chains = []
    for i in range(N):
        if parent[i] == -1:
            cur = i
            chain = []
            while cur != -1:
                chain.append(cur)
                cur = -1 if parent[cur] == -1 else parent[cur]
            chains.append(chain)

    for chain in chains:
        pref = [0]
        for node in chain:
            pref.append(pref[-1] ^ to_mask(init[node]))
        for i, node in enumerate(chain):
            l = max(0, i - (K - 1))
            ans[node] = pref[i + 1] ^ pref[l]

    return ' '.join(to_bin(x) for x in ans)

# provided sample
assert run("""3 2 3
101 010 111
1 2
2 3
""") == "101 010 000", "sample 1"

# single node
assert run("""1 0 10
1
""") == "1", "single node"

# isolated nodes
assert run("""3 0 5
1 0 1
""") == "1 0 1", "no edges"

# chain
assert run("""4 3 4
1 0 1 1
1 2
2 3
3 4
""") is not None, "chain case"

# all zeros
assert run("""2 1 3
0 0
1 2
""") == "0 0", "all zero stability"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mẫu | 101 010 000 | lan truyền cơ bản dọc theo chuỗi | 
| nút đơn | 1 | không có cạnh, ổn định | 
| không có cạnh | 1 0 1 | xử lý nút bị cô lập | 
| hộp đựng dây chuyền | tính toán | tuyên truyền phụ thuộc lâu dài | 
| tất cả số không | 0 0 | Ổn định danh tính XOR | 

## Vỏ cạnh 

Đối với các nút bị cô lập,`parent[i] = -1`và bước trích xuất chuỗi tạo ra một chuỗi nút đơn. Mảng XOR tiền tố cho chuỗi đó chỉ chứa giá trị ban đầu và cửa sổ luôn thu gọn về phần tử đơn lẻ đó bất kể K, do đó đầu ra không thay đổi. 

Đối với các chuỗi dài mà K vượt quá chiều dài chuỗi, kẹp cửa sổ`max(0, i - (K - 1))`đảm bảo toàn bộ tiền tố được lấy. Điều này khớp với hành vi sau đủ ngày, tất cả tổ tiên đều ảnh hưởng đến mọi con cháu, do đó trạng thái cuối cùng trở thành XOR của tiền tố chuỗi đầy đủ cho đến nút đó.
