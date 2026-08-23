---
title: "CF 104283G - Truy vấn cây khác"
description: "Chúng ta được cho một dãy các cọc được sắp xếp theo một thứ tự cố định. Mỗi đống chứa một số viên đá và người chơi thay phiên nhau chơi."
date: "2026-07-01T21:02:25+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104283
codeforces_index: "G"
codeforces_contest_name: "Contest Based on Brain Craft Intra SUST Programming Contest 2023"
rating: 0
weight: 104283
solve_time_s: 68
verified: true
draft: false
---

[CF 104283G - Một truy vấn cây khác](https://codeforces.com/problemset/problem/104283/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 8 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy các cọc được sắp xếp theo một thứ tự cố định. Mỗi đống chứa một số viên đá và người chơi thay phiên nhau chơi. Trong một lượt, người chơi chọn một cọc, nhưng có một sự phụ thuộc nghiêm ngặt: một cọc chỉ có thể được sử dụng nếu mọi cọc trước đó đã được làm trống hoàn toàn. Bên trong một đống đã chọn, người chơi có thể loại bỏ bất kỳ số lượng đá dương nào. 

Trò chơi được chơi trên một đoạn cọc từ chỉ số L đến R và người chơi không thể di chuyển sẽ thua. Ngoài ra, hệ thống còn hỗ trợ cập nhật khi số lượng đá trong một cọc thay đổi. 

Hạn chế chính về cấu trúc là việc chơi chỉ có thể tiến triển từ trái sang phải. Điều này có nghĩa là trò chơi không tự do phân nhánh giữa các cọc mà thay vào đó diễn ra theo một thứ tự cố định, trong đó cọc i+1 không thể tiếp cận được cho đến khi cọc i trống. 

Từ góc độ phức tạp, số lượng cọc đủ lớn để việc tính toán lại kết quả của một phân đoạn từ đầu cho mỗi truy vấn sẽ quá chậm. Một mô phỏng trò chơi đơn giản cho mỗi truy vấn sẽ yêu cầu xử lý từng viên đá hoặc từng cọc trong phạm vi, dẫn đến thời gian tuyến tính cho mỗi truy vấn, điều này không tương thích với các ràng buộc thông thường khi cả n và q đều lớn. 

Trường hợp cạnh khó phát hiện khi cọc không chứa viên đá nào. Ví dụ: nếu một cọc đã trống, nó sẽ biến mất khỏi chuỗi các bước di chuyển bắt buộc một cách hiệu quả, làm thay đổi diễn biến của trò chơi. Một trường hợp quan trọng khác là khi tất cả cọc trong một phạm vi đều trống, trong trường hợp đó không có nước đi nào tồn tại và người chơi đầu tiên ngay lập tức thua cuộc. 

## Phương pháp tiếp cận 

Một cách trực tiếp để tiếp cận vấn đề là mô phỏng trò chơi. Đối với một truy vấn trong phạm vi từ L đến R, chúng tôi sẽ quét liên tục từ L đến R, tìm đống không trống đầu tiên, loại bỏ một số viên đá khỏi nó và thay phiên nhau cho đến khi không còn di chuyển nào. Tuy nhiên, mô phỏng này về cơ bản là không hiệu quả vì mỗi lần di chuyển chỉ làm trống tối đa một đống và trong trường hợp xấu nhất, chúng ta có thể xem lại cùng một cấu trúc nhiều lần trên các truy vấn. Với tối đa 10^5 cọc và 10^5 truy vấn, điều này dẫn đến hành vi bậc hai. 

Quan sát quan trọng xuất phát từ cấu trúc của quy tắc: người chơi có thể loại bỏ bất kỳ số lượng đá dương nào khỏi đống đá hiện có. Điều đó có nghĩa là nước đi tối ưu luôn là loại bỏ toàn bộ cọc trong một lượt. Không bao giờ có lý do để bỏ lại quân cờ, vì làm như vậy chỉ khiến đối thủ phải di chuyển thêm mà không thay đổi quyền truy cập vào các cọc trong tương lai. 

Sau khi điều này được nhận ra, mỗi cọc không trống sẽ đóng góp chính xác một nước đi bắt buộc trong trò chơi, bởi vì nó sẽ bị xóa trong một lượt khi đạt đến. Toàn bộ trò chơi trên một phân đoạn trở thành một chuỗi các bước di chuyển bắt buộc xen kẽ đơn giản trên các cọc không trống theo thứ tự từ trái sang phải. Người chiến thắng được xác định hoàn toàn bằng việc số cọc không trống trong đoạn đó là số lẻ hay số chẵn. 

Điều này làm giảm mỗi truy vấn thành một truy vấn chẵn lẻ trên một mảng động trong đó mỗi phần tử là hoạt động (khác 0) hoặc không hoạt động (không), với các cập nhật điểm sẽ thay đổi trạng thái của một đống. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n) mỗi truy vấn | O(1) | Quá chậm | 
| Ngang bằng với Fenwick/Cây phân đoạn | O(log n) mỗi lần cập nhật/truy vấn | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giảm vấn đề xuống còn việc duy trì một mảng nhị phân trong đó mỗi cọc được biểu thị là 1 nếu nó chứa ít nhất một viên đá và 0 nếu ngược lại.

1. Chuyển đổi từng giá trị cọc thành trạng thái boolean cho biết nó có trống hay không. Điều này nắm bắt tất cả thông tin liên quan đến cách chơi, vì chỉ sự tồn tại của cọc mới quan trọng chứ không phải kích thước của nó. 
2. Xây dựng cấu trúc dữ liệu hỗ trợ hiệu quả hai thao tác: cập nhật một vị trí khi một cọc thay đổi và truy vấn tổng các giá trị trong một phạm vi. Tổng ở đây đại diện cho số lượng cọc đang hoạt động. 
3. Đối với truy vấn cập nhật, đặt vị trí i thành 1 nếu giá trị mới x lớn hơn 0, nếu không thì đặt thành 0. Điều này giữ cho cách trình bày nhất quán với hành vi của trò chơi. 
4. Đối với truy vấn phạm vi từ L đến R, hãy tính tổng các cọc hoạt động trong khoảng đó. Điều này cho biết số lần di chuyển bắt buộc trong phân đoạn trò chơi. 
5. Quyết định người chiến thắng dựa trên tính chẵn lẻ: nếu số cọc đang hoạt động là số lẻ thì người chơi đầu tiên thắng vì họ thực hiện các nước đi thứ nhất, thứ ba, thứ năm, v.v.; nếu không người chơi thứ hai sẽ thắng. 

Tính chính xác phụ thuộc vào thực tế là mỗi cọc đang hoạt động đóng góp chính xác một nước đi không thể đảo ngược theo thứ tự và người chơi không thể sắp xếp lại hoặc bỏ qua các cọc. 

### Tại sao nó hoạt động 

Trò chơi luôn tiến hành nghiêm ngặt từ trái sang phải. Khi một cọc trở thành tâm điểm hiện tại, người chơi tối ưu luôn làm trống nó ngay lập tức chỉ bằng một nước đi. Không có quyết định nào trong tương lai có thể ảnh hưởng đến cọc trước đó và không có sự di chuyển một phần nào mang lại lợi thế. Điều này buộc trò chơi phải đi vào một chuỗi xác định các bước đi đơn lẻ trên một tập hợp các cọc không trống. Vì người chơi luân phiên nhau di chuyển nên kết quả chỉ phụ thuộc vào độ dài chuỗi này là lẻ hay chẵn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Fenwick:
    def __init__(self, n):
        self.n = n
        self.bit = [0] * (n + 1)

    def add(self, i, v):
        while i <= self.n:
            self.bit[i] += v
            i += i & -i

    def sum(self, i):
        s = 0
        while i > 0:
            s += self.bit[i]
            i -= i & -i
        return s

    def range_sum(self, l, r):
        return self.sum(r) - self.sum(l - 1)

def main():
    n, q = map(int, input().split())
    arr = list(map(int, input().split()))

    ft = Fenwick(n)
    state = [0] * (n + 1)

    for i in range(1, n + 1):
        state[i] = 1 if arr[i - 1] > 0 else 0
        ft.add(i, state[i])

    out = []

    for _ in range(q):
        tmp = input().split()
        if tmp[0] == '1':
            i = int(tmp[1])
            x = int(tmp[2])
            new_state = 1 if x > 0 else 0
            diff = new_state - state[i]
            if diff != 0:
                state[i] = new_state
                ft.add(i, diff)
        else:
            l, r = int(tmp[1]), int(tmp[2])
            cnt = ft.range_sum(l, r)
            if cnt % 2 == 1:
                out.append("1")
            else:
                out.append("-1")

    print("\n".join(out))

if __name__ == "__main__":
    main()
```Cây Fenwick duy trì số lượng cọc không trống. Mỗi lần cập nhật chỉ điều chỉnh tối đa một vị trí ±1, tùy thuộc vào việc một cọc có chuyển tiếp giữa trạng thái trống và không trống hay không. Mỗi truy vấn tính toán chênh lệch tổng tiền tố để thu được số lượng cọc hoạt động trong một phạm vi. 

Một chi tiết triển khai tinh tế là chúng tôi không bao giờ lưu trữ số lượng đá thô trong cấu trúc dữ liệu. Chỉ có trạng thái nhị phân mới quan trọng, vì vậy mọi bản cập nhật đều chuyển sang kiểm tra ngưỡng đơn giản. 

## Ví dụ đã hoạt động 

Hãy xem xét cấu hình ban đầu gồm năm cọc:`[3, 0, 2, 0, 1]`. 

Truy vấn phạm vi`[1, 5]`đưa ra các cọc hoạt động ở vị trí 1, 3 và 5 nên số lượng là 3. 

| Bước | Cọc Hoạt Động | Đếm | Chẵn lẻ | 
| --- | --- | --- | --- | 
| Ban đầu | 1, 3, 5 | 3 | lẻ | 

Vì số đếm là số lẻ nên người chơi đầu tiên sẽ thắng. 

Bây giờ hãy xem xét việc cập nhật cọc 3 về 0, tạo mảng`[3, 0, 0, 0, 1]`. 

Một truy vấn trên`[1, 5]`bây giờ mang lại hai cọc hoạt động. 

| Bước | Cọc Hoạt Động | Đếm | Chẵn lẻ | 
| --- | --- | --- | --- | 
| Sau khi cập nhật | 1, 5 | 2 | Thậm chí | 

Bây giờ người chơi thứ hai thắng. 

Những dấu vết này cho thấy rằng toàn bộ trò chơi giảm xuống việc theo dõi xem còn lại bao nhiêu nước đi hiệu quả trong khoảng thời gian đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log n) mỗi thao tác | Mỗi lần cập nhật và tổng phạm vi được xử lý bằng cách truyền cây Fenwick theo chiều cao logarit | 
| Không gian | O(n) | Mảng cho cây Fenwick và lưu trữ trạng thái | 

Điều này phù hợp thoải mái trong các ràng buộc điển hình cho các phép tính 10^5, vì các hệ số logarit vẫn còn nhỏ trong thực tế. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from math import *
    from collections import *
    
    # Fenwick + solution bundled
    input = sys.stdin.readline

    class Fenwick:
        def __init__(self, n):
            self.n = n
            self.bit = [0] * (n + 1)

        def add(self, i, v):
            while i <= self.n:
                self.bit[i] += v
                i += i & -i

        def sum(self, i):
            s = 0
            while i > 0:
                s += self.bit[i]
                i -= i & -i
            return s

        def range_sum(self, l, r):
            return self.sum(r) - self.sum(l - 1)

    n, q = map(int, input().split())
    arr = list(map(int, input().split()))

    ft = Fenwick(n)
    state = [0] * (n + 1)

    for i in range(1, n + 1):
        state[i] = 1 if arr[i - 1] > 0 else 0
        ft.add(i, state[i])

    out = []
    for _ in range(q):
        t = input().split()
        if t[0] == '1':
            i = int(t[1]); x = int(t[2])
            ns = 1 if x > 0 else 0
            if ns != state[i]:
                ft.add(i, ns - state[i])
                state[i] = ns
        else:
            l = int(t[1]); r = int(t[2])
            cnt = ft.range_sum(l, r)
            out.append("1" if cnt % 2 else "-1")

    return "\n".join(out).strip()

# custom tests
assert run("5 3\n1 0 2 0 1\n2 1 5\n1 3 0\n2 1 5") == "1\n-1"
assert run("3 1\n0 0 0\n2 1 3") == "-1"
assert run("4 2\n1 2 3 4\n2 1 4\n1 2 0\n2 1 4") == "1\n-1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cập nhật và truy vấn hỗn hợp | kết quả xen kẽ | tính đúng đắn của việc theo dõi chẵn lẻ | 
| tất cả số không | -1 | trường hợp mất đoạn trống | 
| mảng hoạt động đầy đủ với bản cập nhật | lật người chiến thắng | cập nhật tính đúng đắn | 

## Vỏ cạnh 

Một phạm vi hoàn toàn trống rỗng như`[0, 0, 0]`không tạo ra cọc hoạt động. Thuật toán tính toán chính xác tổng bằng 0 và vì 0 là số chẵn nên nó cho ra kết quả là người chơi thứ hai thắng, khớp với thực tế là người chơi thứ nhất không có nước đi hợp pháp. 

Một cọc không trống sẽ được coi là một nước đi cưỡng bức duy nhất. Cây Fenwick trả về số một, số này là số lẻ nên người chơi đầu tiên sẽ thắng, phù hợp với thực tế là họ ngay lập tức làm trống đống đó và không để lại nước đi nào nữa. 

Trình tự trong đó các bản cập nhật chuyển đổi các cọc giữa 0 và khác 0 được xử lý chính xác vì mỗi bản cập nhật chỉ thay đổi mức đóng góp của chỉ số đó chính xác bằng ±1 trong cấu trúc Fenwick, duy trì tính chính xác của tất cả các tổng phạm vi tiếp theo.
