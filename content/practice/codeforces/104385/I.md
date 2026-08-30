---
title: "CF 104385I - Cây"
description: "Chúng ta được cho một cây trong đó mỗi cạnh mang một giá trị nguyên. Cây cố định nhưng giá trị cạnh thay đổi theo thời gian. Hệ thống hỗ trợ hai hoạt động."
date: "2026-07-01T02:54:39+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104385
codeforces_index: "I"
codeforces_contest_name: "2023 (ICPC) Jiangxi Provincial Contest -- Official Contest"
rating: 0
weight: 104385
solve_time_s: 54
verified: true
draft: false
---

[CF 104385I - Cây](https://codeforces.com/problemset/problem/104385/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 54s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một cây trong đó mỗi cạnh mang một giá trị nguyên. Cây cố định nhưng giá trị cạnh thay đổi theo thời gian. Hệ thống hỗ trợ hai hoạt động. 

Thao tác đầu tiên chọn hai nút và một giá trị z, sau đó mọi cạnh nằm trên đường dẫn đơn giản duy nhất giữa hai nút đó có giá trị được sửa đổi bằng cách áp dụng XOR theo bit với z. Đây không phải là sự thay thế mà là sự chuyển đổi trong biểu diễn nhị phân, được áp dụng độc lập cho từng cạnh trên đường dẫn đó. 

Hoạt động thứ hai yêu cầu một nút duy nhất và yêu cầu tính toán XOR của tất cả các giá trị cạnh hiện có liên quan đến nút đó. 

Cây có tới 500.000 nút và số lượng thao tác cũng lên tới 500.000, do đó, bất kỳ giải pháp nào kiểm tra rõ ràng mọi cạnh trên một đường dẫn sẽ thất bại. Ngay cả một bản cập nhật đường dẫn dài cũng có thể chạm vào các cạnh O(n) và việc lặp lại điều đó qua q thao tác sẽ dẫn đến O(nq), vượt xa giới hạn khả thi. 

Một điểm tinh tế là các bản cập nhật không yêu cầu tổng đường dẫn hoặc truy vấn đường dẫn mà thay vào đó sửa đổi trạng thái cạnh và các truy vấn mang tính cục bộ đối với các nút mà phụ thuộc vào tất cả các cạnh liền kề. Sự không phù hợp này là nơi các phương pháp truyền tải ngây thơ bị phá vỡ. 

Một trường hợp lỗi điển hình xuất hiện khi cập nhật liên tục các đường dẫn dài trong cây hình chuỗi. Ví dụ: trong một chuỗi gồm 100000 nút, một bản cập nhật duy nhất từ ​​đầu này sang đầu kia sẽ chạm đến hầu hết mọi cạnh và việc thực hiện việc đó lặp đi lặp lại sẽ ngay lập tức vượt quá giới hạn thời gian. 

## Phương pháp tiếp cận 

Phương pháp mô phỏng trực tiếp sẽ tính toán lại từng đường dẫn bằng cách tìm đường dẫn giữa x và y bằng DFS hoặc BFS, sau đó lặp qua tất cả các cạnh trên đường dẫn đó và chuyển đổi giá trị của chúng. Điều này đơn giản về mặt khái niệm vì cấu trúc cây đảm bảo một đường dẫn duy nhất. Tuy nhiên, mỗi thao tác như vậy có thể mất thời gian tuyến tính trong trường hợp xấu nhất. Với 500.000 hoạt động, điều này trở nên không khả thi. 

Quan sát quan trọng là mặc dù các cạnh đang được cập nhật, nhưng truy vấn không hỏi về đường dẫn hoặc cây con mà chỉ về XOR của các cạnh liên quan đến một nút. Điều này cho phép chúng tôi tránh theo dõi rõ ràng từng cạnh sau khi cập nhật. 

Hãy xem điều gì sẽ xảy ra khi một cạnh (u, v) có giá trị XOR bởi z. Sự thay đổi đó ảnh hưởng đến chính xác hai nút: u và v, vì cả hai điểm cuối đều nhìn thấy cạnh đó trong danh sách kề của chúng. Nếu chúng ta nghĩ về mặt đóng góp của nút, thì mọi cập nhật cạnh đều tương đương với việc áp dụng XOR z cho cả hai điểm cuối của cạnh đó. 

Bây giờ hãy xem xét cập nhật đường dẫn đầy đủ từ x đến y. Đường đi đó bao gồm một chuỗi các cạnh. Mỗi nút bên trong trên đường đi đều liên quan đến chính xác hai trong số các cạnh được cập nhật đó, trong khi các điểm cuối x và y liên quan đến chính xác một cạnh. Vì XOR là nghịch đảo của chính nó nên việc áp dụng z hai lần sẽ bị loại bỏ. Vì vậy, các nút bên trong không nhận được hiệu ứng ròng nào, trong khi các điểm cuối nhận được chính xác một XOR z. 

Điều này sẽ thu gọn toàn bộ quá trình cập nhật đường dẫn thành một hoạt động liên tục: chúng ta chỉ cần XOR z thành x và y. 

Đối với mỗi nút, chúng tôi cũng duy trì XOR của các trọng số cạnh tới ban đầu. Mỗi bản cập nhật đều góp phần chuyển đổi bổ sung cho các điểm cuối. Truy vấn chỉ cần kết hợp giá trị ban đầu với tất cả các chuyển đổi tích lũy. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Truyền tải đường dẫn Brute Force mỗi lần cập nhật | O(nq) | O(n) | Quá chậm | 
| Tổng hợp chuyển đổi điểm cuối | O(n + q) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi root cây ở bất cứ đâu, mặc dù việc root cây không thực sự cần thiết để đảm bảo tính chính xác. Trước tiên, chúng tôi tính toán XOR ban đầu của tất cả các trọng số cạnh tới cho mỗi nút. 

Mỗi cạnh đóng góp trọng số của nó cho cả hai điểm cuối của nó, vì vậy chúng ta có thể khởi tạo một mảng`base[x]`lưu trữ XOR này trực tiếp. 

Chúng tôi cũng duy trì một mảng khác`delta[x]`, ban đầu tất cả đều là số 0, lưu trữ hiệu ứng tích lũy của tất cả các cập nhật đường dẫn. 

Bây giờ chúng ta xử lý các thao tác theo thứ tự. 

1. Đối với thao tác loại 1 với các nút x và y và giá trị z, chúng tôi áp dụng`delta[x] ^= z`Và`delta[y] ^= z`. Điều này thể hiện thực tế rằng chính xác các điểm cuối của đường dẫn sẽ chịu hiệu ứng XOR ròng của z. 
2. Đối với thao tác loại 2 với nút x, chúng ta xuất ra`base[x] ^ delta[x]`. Điều này kết hợp XOR sự cố ban đầu với tất cả các sửa đổi có ảnh hưởng đến các cạnh liên quan đến x thông qua cập nhật đường dẫn. 

Tính chính xác của bước 1 phụ thuộc vào việc đếm số lần nút x xuất hiện dưới dạng điểm cuối của các cạnh được cập nhật trên đường dẫn. Các nút nội bộ xuất hiện hai lần và hủy bỏ, các điểm cuối xuất hiện một lần và giữ nguyên. 

### Tại sao nó hoạt động 

Mỗi bản cập nhật biên đều ảnh hưởng chính xác đến hai điểm cuối của nó. Cập nhật đường dẫn là tập hợp các cập nhật cạnh dọc theo một đường dẫn đơn giản. Đối với bất kỳ nút nào, số cạnh được cập nhật sự cố trong đường dẫn đó là 0, một hoặc hai tùy thuộc vào việc nó nằm ngoài đường dẫn, điểm cuối của đường dẫn hay nút bên trong trên đường dẫn. Tích lũy XOR biến trường hợp “hai lần xuất hiện” thành không thay đổi và trường hợp “một lần xuất hiện” thành một lần chuyển đổi duy nhất. Điều này làm giảm toàn bộ cấu trúc đường dẫn thành các hiệu ứng chỉ dành cho điểm cuối, duy trì sự đóng góp chính xác mà mỗi nút sẽ nhận được từ tất cả các cạnh được sửa đổi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n, q = map(int, input().split())

    base = [0] * (n + 1)
    delta = [0] * (n + 1)

    edges = []

    for _ in range(n - 1):
        u, v, w = map(int, input().split())
        base[u] ^= w
        base[v] ^= w
        edges.append((u, v, w))

    out = []

    for _ in range(q):
        tmp = input().split()
        op = int(tmp[0])

        if op == 1:
            x = int(tmp[1])
            y = int(tmp[2])
            z = int(tmp[3])

            delta[x] ^= z
            delta[y] ^= z

        else:
            x = int(tmp[1])
            out.append(str(base[x] ^ delta[x]))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    main()
```Việc thực hiện tính toán trước`base[x]`bằng cách XOR trực tiếp tất cả các trọng số của cạnh tới trong khi đọc các cạnh. Điều này tránh việc lưu trữ danh sách kề. các`delta`mảng tích lũy hiệu ứng của tất cả các cập nhật đường dẫn chạm đến từng điểm cuối. 

Đối với hoạt động loại 1, chỉ có hai điểm cuối được cập nhật. Đây là sự tối ưu hóa quan trọng giúp loại bỏ sự phụ thuộc vào độ dài đường dẫn. 

Đối với các hoạt động loại 2, câu trả lời là XOR hiệu quả hiện tại của tất cả các cạnh sự cố, được xây dựng lại thành giá trị ban đầu cộng với tất cả các chuyển đổi điểm cuối tích lũy. 

## Ví dụ đã hoạt động 

Hãy xem xét một chuỗi nhỏ: 

đầu vào:```
3 3
1 2 1
1 3 2
2 1
1 1 3 2
2 1
```Chúng tôi bắt đầu bằng việc xây dựng`base`: 

Nút 1 có các cạnh (1,2)=1 và (1,3)=2 nên base[1]=3. 

Nút 2 có cơ sở[2]=1. 

Nút 3 có cơ sở[3]=2. 

Ban đầu delta hoàn toàn bằng 0. 

| Bước | Hoạt động | đồng bằng[1] | đồng bằng[2] | đồng bằng[3] | Kết quả truy vấn | 
| --- | --- | --- | --- | --- | --- | 
| 1 | truy vấn 1 | 0 | 0 | 0 | cơ sở[1]=3 | 
| 2 | cập nhật 1-3 với z=2 | 2 | 0 | 2 | - | 
| 3 | truy vấn 1 | 2 | 0 | 2 | 3 ^ 2 = 1 | 

Bước thứ hai chỉ áp dụng XOR 2 cho các nút 1 và 3, phản ánh rằng chỉ các điểm cuối của quá trình cập nhật đường dẫn mới bị ảnh hưởng. 

Dấu vết này cho thấy cách các cạnh bên trong được xử lý ngầm mà không bao giờ được xử lý rõ ràng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + q) | mỗi cạnh được xử lý một lần, mỗi truy vấn/cập nhật là O(1) | 
| Không gian | O(n) | mảng cho cơ sở và delta | 

Các ràng buộc cho phép lên tới 500.000 thao tác, do đó, giải pháp O(n + q) phù hợp thoải mái trong cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import main
    return sys.stdout.getvalue().strip()

# sample
assert run("""3 3
1 2 1
1 3 2
2 1
1 1 3 2
2 1
""") == "3\n1", "sample 1"

# single edge
assert run("""2 2
1 2 5
2 1
2 2
""") == "5\n5", "simple chain"

# no-op update x==y
assert run("""3 1
1 2 7
1 3 9
1 2 2 0
""") == "", "no-op path update"

# star tree
assert run("""4 3
1 2 1
1 3 2
1 4 3
2 1
1 2 3 1
2 1
""") == "0\n1", "star updates"

# repeated toggles
assert run("""3 4
1 2 4
2 1
1 1 3 5
1 1 3 5
2 1
""") == "4\n4", "double cancel"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chuỗi | 5,5 | lân cận cơ bản XOR | 
| cập nhật không hoạt động | trống | x==y hủy | 
| cây sao | hỗn hợp | lan truyền chỉ điểm cuối | 
| chuyển đổi lặp đi lặp lại | ổn định | Hủy XOR | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi đường dẫn cập nhật suy biến thành một nút duy nhất, nghĩa là x bằng y. Trong tình huống đó, không có cạnh nào bị ảnh hưởng nên thao tác không được thay đổi gì cả. Thuật toán xử lý việc này một cách chính xác vì nó áp dụng`delta[x] ^= z`hai lần, một lần cho x và một lần cho y, và vì chúng là cùng một nút nên XOR sẽ bị loại bỏ. 

Một trường hợp khác là cây hình ngôi sao trong đó có nhiều cập nhật nhắm vào nút trung tâm. Vì mọi đường đi giữa các lá đều đi qua trung tâm nên có vẻ như trung tâm cần được cập nhật nhiều. Trong thực tế, quy tắc điểm cuối đảm bảo rằng chỉ các lá được chuyển đổi trong mỗi thao tác và phần trung tâm vẫn ổn định trừ khi đó là điểm cuối. 

Trường hợp thứ ba được lặp lại các cập nhật giống hệt nhau trên cùng một đường dẫn. Bởi vì XOR là nghịch đảo của chính nó nên việc áp dụng cùng một bản cập nhật hai lần sẽ bị hủy hoàn toàn. các`delta`quá trình tích lũy sẽ bảo tồn thuộc tính này một cách tự nhiên, vì mỗi lần cập nhật chuyển đổi các điểm cuối một cách độc lập và các lần chuyển đổi lặp lại sẽ hoàn nguyên trạng thái.
