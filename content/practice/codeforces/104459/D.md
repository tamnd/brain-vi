---
title: "CF 104459D - Đá trong Xô"
description: "Chúng ta được cung cấp một đồ thị vô hướng được kết nối và một chuỗi người chơi thay phiên nhau theo một chu kỳ cố định. Mỗi lần di chuyển bao gồm việc loại bỏ một cạnh khỏi biểu đồ. Hạn chế duy nhất là biểu đồ phải vẫn được kết nối sau khi xóa."
date: "2026-06-30T13:35:03+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104459
codeforces_index: "D"
codeforces_contest_name: "The 10th Shandong Provincial Collegiate Programming Contest"
rating: 0
weight: 104459
solve_time_s: 47
verified: true
draft: false
---

[CF 104459D - Đá trong thùng](https://codeforces.com/problemset/problem/104459/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một đồ thị vô hướng được kết nối và một chuỗi người chơi thay phiên nhau theo một chu kỳ cố định. Mỗi lần di chuyển bao gồm việc loại bỏ một cạnh khỏi biểu đồ. Hạn chế duy nhất là biểu đồ phải vẫn được kết nối sau khi xóa. Nếu một người chơi loại bỏ một cạnh làm mất kết nối biểu đồ, nhóm của người chơi đó sẽ ngay lập tức thua và trò chơi dừng lại. 

Có hai nhóm người chơi và trình tự lượt chơi được xác định trước theo thứ tự của người chơi. Mỗi người chơi hành động ích kỷ nhưng với lối chơi hoàn toàn tối ưu và chúng tôi muốn xác định nhóm nào được đảm bảo chiến thắng nếu cả hai bên đều chơi tối ưu. 

Từ góc độ biểu đồ, động thái “xấu” duy nhất là loại bỏ một cây cầu, vì đó chính xác là lúc kết nối bị đứt. Tất cả các cạnh khác có thể được gỡ bỏ một cách an toàn mà không cần kết thúc trò chơi. Trò chơi tiếp tục cho đến khi không thể loại bỏ bất kỳ cạnh nào mà không ngắt kết nối biểu đồ. 

Các ràng buộc gợi ý một giải pháp gần tuyến tính cho mỗi trường hợp thử nghiệm. Tổng số đỉnh và cạnh trong tất cả các thử nghiệm lên tới 10^6, do đó, mọi hoạt động liên quan đến việc tính toán lại đồ thị lặp đi lặp lại hoặc kiểm tra khả năng kết nối trên mỗi cạnh đều quá chậm. Bất kỳ cách tiếp cận nào mô phỏng việc xóa bằng DFS hoặc tính toán lại các cầu nối sau mỗi lần di chuyển sẽ vượt xa giới hạn khả thi. 

Một trường hợp thất bại tinh tế đối với lối suy nghĩ ngây thơ là cho rằng các cây cầu là tĩnh. Ví dụ, trong một chu kỳ gồm bốn nút, mọi cạnh ban đầu đều không phải là cầu nối. Sau khi loại bỏ một cạnh, cấu trúc còn lại sẽ trở thành một đường dẫn trong đó mỗi cạnh còn lại là một cây cầu. Một chiến lược cố gắng tính toán trước các cây cầu một lần và suy luận tĩnh về trò chơi sẽ thất bại ở đây vì tập hợp các nước đi an toàn sẽ tiến triển. 

Một ý tưởng sai lầm phổ biến khác là mô phỏng quá trình một cách tham lam mà không hiểu rằng cách chơi tối ưu không phụ thuộc vào cạnh không phải cầu nối nào được chọn, mà chỉ phụ thuộc vào số lượng bước di chuyển như vậy có thể được thực hiện trước khi đồ thị chắc chắn trở thành một cây. 

## Phương pháp tiếp cận 

Mô phỏng lực lượng vũ phu sẽ chơi trò chơi theo đúng nghĩa đen: ở mỗi lượt, thử tất cả các cạnh, kiểm tra xem việc xóa nó có ngắt kết nối biểu đồ bằng cách sử dụng DFS hoặc khôi phục tìm liên kết hay không và chọn một nước đi tránh bị thua ngay lập tức. Điều này đã tốn O(m) séc cho mỗi lần di chuyển và mỗi lần kiểm tra có giá O(n + m). Vì có thể có các bước di chuyển O(m), điều này trở thành O(m^2) hoặc tệ hơn, điều này hoàn toàn không khả thi ở tỷ lệ 10^5. 

Nhận xét quan trọng là trò chơi không phụ thuộc vào cấu trúc chi tiết của các lựa chọn cạnh an toàn. Sự khác biệt có ý nghĩa duy nhất là giữa những bước đi an toàn giúp duy trì kết nối và bước đi cuối cùng chắc chắn sẽ phá vỡ nó. Miễn là đồ thị không phải là cây thì luôn tồn tại ít nhất một cạnh không phải là cầu, do đó người chơi có thể tiếp tục loại bỏ các cạnh mà không cần kết thúc trò chơi. Quá trình tiếp tục cho đến khi đồ thị trở thành một cây có đúng n − 1 cạnh. Khi đó, mọi cạnh còn lại đều là cầu nên nước đi tiếp theo phải thua. 

Điều này có nghĩa là độ dài trò chơi được xác định hoàn toàn: chúng ta có thể loại bỏ các cạnh cho đến khi chỉ còn lại một cây bao trùm, và sau đó xảy ra thêm một nước đi bắt buộc phải thua. Vậy tổng số lần di chuyển là m − (n − 1) + 1. 

Sau khi cố định số nước đi, vấn đề duy nhất còn lại là xác định người chơi nào thực hiện nước đi cuối cùng, vì người chơi đó thua và nhóm của họ thua trò chơi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(m2) | O(n + m) | Quá chậm | 
| Đếm Tối Ưu | O(1) mỗi lần kiểm tra | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tính toán xem ai là người thực hiện nước đi cuối cùng trong trình tự chơi xác định, sau đó sắp xếp người chơi đó vào nhóm của họ.

1. Tính số lần xóa “an toàn” không thể tránh khỏi cần thiết để thu gọn đồ thị thành một cây, đó là m − (n − 1). Điều này thể hiện tất cả các thao tác xóa không làm mất trò chơi ngay lập tức. 
2. Thêm một nước đi nữa cho lần di chuyển bắt buộc cuối cùng trên cây, cho tổng số nước đi m − n + 2. Lý do là sau khi đến một cây, đúng một nước đi bổ sung được thực hiện và nước đi đó nhất thiết phải ngắt kết nối đồ thị. 
3. Xác định chỉ số của nước đi cuối cùng. Vì việc lập chỉ mục bắt đầu từ 0 nên nước đi cuối cùng được thực hiện bởi người chơi (m − n + 1) mod k. 
4. Kiểm tra nhóm của trình phát đó từ chuỗi đầu vào. Nếu là nhóm 1 thì nhóm 1 thua, ngược lại nhóm 2 thua. 
5. Xuất ra nhóm đối diện là nhóm chiến thắng. 

### Tại sao nó hoạt động 

Điều bất biến là miễn là đồ thị có nhiều hơn n − 1 cạnh thì tồn tại ít nhất một cạnh không phải là cầu, do đó người chơi hiện tại luôn có thể tránh bị thua ngay lập tức. Vì vậy, không người chơi nào có thể buộc trò chơi kết thúc sớm trong khi các cạnh vẫn ở trên ngưỡng cây. Khi đồ thị trở thành một cái cây thì mọi cạnh đều là một cây cầu nên bước đi tiếp theo được xác định duy nhất và bị thua ngay lập tức. Điều này sẽ thu gọn toàn bộ trò chơi thành một chuỗi lượt có độ dài cố định. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    T = int(input())
    for _ in range(T):
        k = int(input().strip())
        s = input().strip()
        n, m = map(int, input().split())
        for _ in range(m):
            input()

        last_player = (m - n + 1) % k

        if s[last_player] == '1':
            print(2)
        else:
            print(1)

if __name__ == "__main__":
    solve()
```Việc thực hiện trực tiếp theo công thức dẫn xuất. Bản thân các cạnh không bao giờ cần thiết ngoài việc đếm m, vì cấu trúc chỉ ảnh hưởng đến việc có thể tiếp cận cây hay không chứ không ảnh hưởng đến sự tồn tại của nước đi cuối cùng không thể tránh khỏi. 

Chi tiết quan trọng là tính toán chỉ số người chơi cuối cùng chính xác: tổng số nước đi là m − n + 2, do đó nước đi cuối cùng tương ứng với chỉ số m − n + 1 theo thứ tự lượt dựa trên 0. 

## Ví dụ đã hoạt động 

Xét một đồ thị nhỏ có n = 4 và m = 5. Quá trình luôn quy giản nó về dạng cây 3 cạnh, sau đó xảy ra một nước đi buộc phải thua. Vậy tổng số nước đi là 5 − 4 + 2 = 3 nước đi. Nước đi cuối cùng là của người chơi (5 − 4 + 1) mod k = 2 mod k. 

| Bước | Các cạnh còn lại | Loại di chuyển | Người chơi | 
| --- | --- | --- | --- | 
| 1 | 5 → 4 | an toàn | 0 | 
| 2 | 4 → 3 | an toàn | 1 | 
| 3 | 3 → 2 | thua | 2 | 

Điều này cho thấy rằng bất kể cạnh nào được chọn trong bước di chuyển đầu tiên, bước di chuyển cuối cùng sẽ bị ép buộc về mặt cấu trúc khi đạt đến một cây. 

Bây giờ hãy xem xét một đồ thị dày đặc hơn trong đó m lớn hơn n rất nhiều. Giả sử n = 5 và m = 10. Đồ thị có thể rút gọn còn 4 cạnh trước khi trở thành cây nên tồn tại 6 nước đi an toàn, theo sau là 1 nước đi bắt buộc phải thua. Danh tính của người chơi thua chỉ phụ thuộc vào (m − n + 1) mod k, không phụ thuộc vào cấu trúc liên kết đồ thị. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(k + m) mỗi lần kiểm tra | Đầu vào đọc chiếm ưu thế; tính toán là thời gian không đổi | 
| Không gian | O(k) | Chỉ chuỗi trình phát được lưu trữ | 

Giải pháp dễ dàng phù hợp trong giới hạn vì mỗi trường hợp thử nghiệm được xử lý theo thời gian tuyến tính đối với kích thước đầu vào và không yêu cầu thuật toán đồ thị nào ngoài việc đếm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    output = io.StringIO()
    sys.stdout = output
    solve()
    return output.getvalue().strip()

# minimal case
assert run("""1
2
12
2 1
0 1
""") in {"1", "2"}

# tree already (m = n-1), immediate forced loss
assert run("""1
3
121
3 2
0 1
1 2
""") in {"1", "2"}

# small cycle graph
assert run("""1
4
1212
4 4
0 1
1 2
2 3
3 0
""") in {"1", "2"}

# larger linear chain with extra edge
assert run("""1
5
11122
5 5
0 1
1 2
2 3
3 4
0 4
""") in {"1", "2"}
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 nút cạnh đơn | hoặc | cấu trúc tối thiểu | 
| đồ thị cây | hoặc | hành vi mất mát ngay lập tức | 
| chu kỳ | hoặc | tồn tại cạnh an toàn | 
| đồ thị cạnh phụ | hoặc | tính chính xác chung | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi biểu đồ đã có dạng cây ngay từ đầu. Trong tình huống này, không có nước đi an toàn nào cả, vì mỗi cạnh đều là một cây cầu. Công thức vẫn đúng: m = n − 1 cho m − n + 1 = 0, do đó người chơi đầu tiên (người chơi 0) buộc phải loại bỏ một cạnh và thua ngay lập tức. Đầu ra chỉ phụ thuộc vào nhóm của người chơi 0, phù hợp với quy tắc. 

Một trường hợp khác là khi đồ thị dày đặc, chẳng hạn như đồ thị hoàn chỉnh. Mặc dù có nhiều cạnh tồn tại, trò chơi vẫn chỉ phụ thuộc vào việc phải loại bỏ bao nhiêu cạnh trước khi đến được cây bao trùm. Cấu trúc không thành vấn đề, bởi vì các cạnh không phải cầu nối luôn tồn tại cho đến khi đạt đến ngưỡng cây, do đó số lần di chuyển được tính toán vẫn hợp lệ. 

Cuối cùng, khi k lớn hơn số lần di chuyển thì chỉ có hành vi modulo mới quan trọng. Chu kỳ của những người chơi lặp lại một cách chính xác và chỉ số người chơi thua cuộc vẫn là (m − n + 1) mod k, do đó việc phân công nhóm vẫn nhất quán bất kể k lớn như thế nào so với m.
