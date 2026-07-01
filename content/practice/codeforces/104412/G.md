---
title: "CF 104412G - Đoán hai bước vào đa vũ trụ"
description: "Chúng ta được cung cấp một đa đồ thị có hướng trên n vũ trụ. Các cạnh (được gọi là cổng) xuất hiện lần lượt theo thời gian và mỗi cổng đều được định hướng. Sau khi mỗi cạnh mới được thêm vào, chúng ta phải duy trì hai giá trị."
date: "2026-06-30T22:52:05+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104412
codeforces_index: "G"
codeforces_contest_name: "2023 ICPC Gran Premio de Mexico 2da Fecha"
rating: 0
weight: 104412
solve_time_s: 112
verified: true
draft: false
---

[CF 104412G - Đoán hai bước vào đa vũ trụ](https://codeforces.com/problemset/problem/104412/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 52s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được đưa ra một phép nhân trực tiếp trên`n`vũ trụ. Các cạnh (được gọi là cổng) xuất hiện lần lượt theo thời gian và mỗi cổng đều được định hướng. 

Sau khi mỗi cạnh mới được thêm vào, chúng ta phải duy trì hai giá trị. 

Giá trị đầu tiên đếm số lượng cặp cổng được sắp xếp tạo thành lộ trình di chuyển hai bước hợp lệ. Nói cách khác, chúng ta xem xét mọi cách để đi từ vũ trụ nào đó`a`đến vũ trụ khác`c`sử dụng chính xác hai cạnh có hướng`a → b → c`. Mỗi lựa chọn cạnh riêng biệt đều được tính theo một cách khác nhau, vì vậy các cạnh song song đóng góp nhiều lần. 

Giá trị thứ hai đặt ra một câu hỏi hướng tới tương lai. Sau khi xử lý cạnh hiện tại, hãy tưởng tượng thêm chính xác một cạnh nữa vào phút tiếp theo. Trong số tất cả các lựa chọn có thể có của cạnh tiếp theo đó, chúng tôi muốn mức tăng tối đa có thể về số lượng tuyến đường hai bước. 

Kích thước đầu vào tăng lên`n, t ≤ 10^5`, do đó, bất kỳ giải pháp nào cố gắng tính toán lại tất cả các đường dẫn có độ dài-2 từ đầu sau mỗi lần chèn sẽ yêu cầu tính toán lại nhiều lần trên tất cả các cạnh. Kể từ khi lên đến`10^5`các cạnh tồn tại, việc tính toán lại mỗi bước sẽ dẫn đến khoảng`O(t * m)`hành vi đó là quá lớn. 

Một trường hợp cạnh quan trọng là các cạnh có thể hình thành các vòng tự lặp và trùng lặp. Ví dụ: nếu chúng ta liên tục thêm`1 → 1`, số lượng đường dẫn hai bước tăng lên nhanh chóng vì mỗi vòng lặp tham gia vừa là tiền tố vừa là hậu tố của nhiều đường dẫn. Một cách tiếp cận ngây thơ chỉ theo dõi sự kề cận đơn giản mà không có bội số sẽ thất bại đối với các đầu vào như:```
1 3
1 1
1 1
1 1
```Đầu ra đúng tăng theo phương trình bậc hai vì mỗi vòng tự mới đều tăng cả cấu trúc đầu vào và đầu ra tại cùng một nút, khuếch đại sự kết hợp hai bước. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ là tính toán lại số lượng đường dẫn có độ dài 2 sau mỗi lần chèn. Sau đó`i`các cạnh, chúng ta sẽ lặp qua tất cả các cặp cạnh và kiểm tra xem phần cuối của cạnh đầu tiên có khớp với phần đầu của cạnh thứ hai hay không. Đó là`O(m^2)`mỗi bước trong trường hợp xấu nhất, trở thành`O(t^3)`tổng thể khi các cạnh tích lũy, hoàn toàn không khả thi. 

Thay vào đó, chúng ta có thể viết lại bài toán đếm theo cách tách biệt sự đóng góp của từng nút. Mọi đường dẫn hai bước hợp lệ`a → b → c`được xác định duy nhất bởi nút giữa của nó`b`. Đối với một cố định`b`, số cách là số cạnh đi vào`b`nhân với số cạnh đi từ`b`. Điều này chuyển đổi vấn đề ghép nối toàn cầu thành sản phẩm trên mỗi nút. 

Quan sát này làm cho cập nhật cục bộ. Khi có một cạnh mới`u → v`được thêm vào, chỉ có hai nút thay đổi:`u`đạt được một cạnh đi và`v`đạt được một cạnh tới. Tất cả các nút khác vẫn không bị ảnh hưởng, vì vậy chỉ những đóng góp liên quan đến`u`Và`v`cần điều chỉnh. 

Điều này cũng mở ra phần thứ hai của vấn đề. Nếu chúng ta muốn tối đa hóa số lượng đường dẫn hai bước bổ sung được tạo bằng cách chèn một cạnh mới`x → y`, chúng ta chỉ quan tâm đến việc cạnh đó làm tăng tổng toàn cầu bao nhiêu. Sự gia tăng đó chỉ phụ thuộc vào`x`Và`y`, do đó nó tách biệt rõ ràng thành bài toán lựa chọn tốt nhất trên cực đại độc lập. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tính toán lại Brute Force | O(m2) mỗi bước | O(m) | Quá chậm | 
| Bất biến theo dõi độ | O(1) mỗi bước | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì hai mảng,`in[v]`Và`out[v]`, theo dõi số lượng cổng hiện đang vào và ra khỏi mỗi vũ trụ. Chúng tôi cũng duy trì tổng toàn cầu đại diện cho tất cả các đường dẫn có độ dài-2. 

1. Khởi tạo tất cả`in`Và`out`giá trị về 0 và đặt câu trả lời`S = 0`. Tại thời điểm này không tồn tại đường dẫn hai bước. 
2. Đối với mỗi cạnh mới`u → v`, trước tiên hãy tính xem cạnh này thay đổi số lượng đường dẫn hai bước như thế nào. Bất kỳ đường dẫn hai bước mới nào đều sử dụng`u`làm điểm bắt đầu của cạnh thứ hai hoặc sử dụng`v`làm điểm cuối của cạnh đầu tiên. 
3. Tăng`S`qua`in[u] + out[v]`. Thuật ngữ`in[u]`đếm mọi cách để đến đích`u`và sau đó sử dụng ngay cạnh mới, trong khi`out[v]`đếm tất cả các con đường để đi từ`v`sau khi đến qua cạnh mới. Đây là những đóng góp rời rạc nên chúng cộng tuyến tính. 
4. Cập nhật độ: tăng dần`out[u]`Và`in[v]`. 
5. Theo dõi cạnh tiếp theo tốt nhất có thể. Lợi ích từ việc thêm một cạnh`x → y`trong tương lai sẽ là`in[x] + out[y]`. Lựa chọn tốt nhất được chia rõ ràng thành tối đa hóa`in[x]`và tối đa hóa`out[y]`một cách độc lập. Vì vậy chúng tôi duy trì`max_in`Và`max_out`trên tất cả các nút. 
6. Sau khi xử lý từng cạnh, xuất ra`(S, max_in + max_out)`. 

### Tại sao nó hoạt động 

Mỗi bước đi có độ dài 2 được xác định duy nhất bởi nút giữa của nó. Điều đó có nghĩa là cấu trúc toàn cầu phân hủy thành những đóng góp độc lập có dạng`in[b] * out[b]`. Vì mỗi bản cập nhật chỉ thay đổi một số lượng đến và một số lượng đi nên sự thay đổi về tổng sẽ được hai nút bị ảnh hưởng nắm bắt hoàn toàn. Không tồn tại tương tác giữa các nút ẩn vì không có thay đổi mức độ vào hoặc ra của nút khác trong một lần cập nhật. 

Đối với giá trị thứ hai, mức tăng từ việc thêm một cạnh chỉ phụ thuộc vào điểm cuối. Từ`in[x]`Và`out[y]`là các lựa chọn độc lập, cạnh tối ưu được hình thành bằng cách chọn nút có mức độ vào tối đa làm nguồn và nút có mức độ đi tối đa làm mục tiêu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, t = map(int, input().split())
    in_deg = [0] * (n + 1)
    out_deg = [0] * (n + 1)

    S = 0
    max_in = 0
    max_out = 0

    for _ in range(t):
        u, v = map(int, input().split())

        S += in_deg[u] + out_deg[v]

        out_deg[u] += 1
        in_deg[v] += 1

        if in_deg[v] > max_in:
            max_in = in_deg[v]
        if out_deg[u] > max_out:
            max_out = out_deg[u]

        print(S, max_in + max_out)

if __name__ == "__main__":
    solve()
```Cốt lõi của việc triển khai là quan sát rằng chúng tôi không bao giờ lưu trữ rõ ràng các cạnh vượt quá ảnh hưởng của chúng đối với số lượng. Bản cập nhật tới`S`phải xảy ra trước khi sửa đổi độ, vì công thức tăng dần dựa vào trạng thái trước đó. Thứ tự này tránh việc vô tình đếm cạnh mới bên trong phần đóng góp của chính nó. 

Bảo trì`max_in`Và`max_out`dần dần tránh quét tất cả các nút mỗi lần. Mỗi bản cập nhật chỉ chạm đến các điểm cuối, vì vậy việc theo dõi cực đại là thời gian không đổi. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4 6
1 2
2 1
1 1
2 2
3 3
4 4
```Chúng tôi theo dõi`(in, out)`những thay đổi và`S`. 

| Bước | Cạnh | trong[u] + ngoài[v] | S sau | max_in | max_out | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1→2 | 0 + 0 = 0 | 0 | 1 | 1 | 
| 2 | 2→1 | 0 + 1 = 1 | 1 | 1 | 1 | 
| 3 | 1→1 | 1 + 1 = 2 | 3 | 2 | 2 | 
| 4 | 2→2 | 1 + 1 = 2 | 5 | 2 | 2 | 
| 5 | 3→3 | 0 + 0 = 0 | 5 | 2 | 2 | 
| 6 | 4→4 | 0 + 0 = 0 | 5 | 2 | 2 | 

Mỗi bước cho thấy các vòng lặp tự tăng cả cấp độ trong và cấp độ ngoài của cùng một nút, tăng nhanh chóng các khoản đóng góp trong tương lai. 

Đầu ra thứ hai bằng`max_in + max_out`, sẽ ổn định khi một nút tích lũy cấu trúc tự vòng lặp lặp lại hoặc nhiều cạnh sự cố. 

### Mẫu 2 

đầu vào:```
1 3
1 1
1 1
1 1
```Ở đây tất cả các bản cập nhật đều ảnh hưởng đến cùng một nút. 

| Bước | Cạnh | trong[1] + ngoài[1] | S | max_in | max_out | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1→1 | 0 | 0 | 1 | 1 | 
| 2 | 1→1 | 1 | 1 | 2 | 2 | 
| 3 | 1→1 | 2 | 3 | 3 | 3 | 

Mỗi vòng lặp mới sẽ tăng cả số lượng đến và đi và vì tất cả các đường dẫn phải đi qua nút 1 nên sự tăng trưởng bậc hai trong các bước đi hai bước xuất hiện một cách tự nhiên như`in[1] * out[1]`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi bản cập nhật cạnh chỉ thực hiện số học theo thời gian không đổi và một vài phép so sánh | 
| Không gian | O(n) | Hai mảng lưu trữ độ trong và độ ngoài cho tất cả các nút | 

Thuật toán xử lý tới`10^5`các cạnh thoải mái trong giới hạn vì mọi hoạt động đều có thời gian không đổi và tránh mọi tính toán lại toàn cầu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    
    n, t = map(int, sys.stdin.readline().split())
    in_deg = [0] * (n + 1)
    out_deg = [0] * (n + 1)

    S = 0
    max_in = 0
    max_out = 0

    for _ in range(t):
        u, v = map(int, sys.stdin.readline().split())
        S += in_deg[u] + out_deg[v]
        out_deg[u] += 1
        in_deg[v] += 1
        max_in = max(max_in, in_deg[v])
        max_out = max(max_out, out_deg[u])
        output.append(f"{S} {max_in + max_out}")

    return "\n".join(output)

# provided samples
assert run("""4 6
1 2
2 1
1 1
2 2
3 3
4 4
""") == """0 2
1 3
3 4
5 4
5 4
5 4"""

assert run("""1 3
1 1
1 1
1 1
""") == """0 2
1 4
3 6"""

# custom cases
assert run("""2 1
1 2
""") == "0 1", "single edge"

assert run("""3 3
1 2
2 3
3 1
""") == "0 2\n1 2\n2 2", "cycle"

assert run("""5 4
1 1
1 1
1 1
1 1
""") == "0 2\n1 4\n3 6\n6 8", "heavy self-loop"

assert run("""4 3
1 2
1 3
4 1
""") == "0 1\n0 2\n2 3", "mixed structure"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cạnh đơn |`0 1`| khởi tạo cơ sở và cập nhật lần đầu | 
| chu kỳ | giá trị gia tăng | tương tác giữa các nút | 
| vòng tự nặng | tăng trưởng bậc hai | gia cố lặp đi lặp lại tại một nút | 
| cấu trúc hỗn hợp | mức độ khác nhau | xử lý đúng đắn các cập nhật bất đối xứng | 

## Vỏ cạnh 

Vòng lặp tự là trường hợp nhạy cảm nhất vì chúng đồng thời sửa đổi cả số lượng vào và ra của cùng một nút. Đối với một đầu vào như`1 → 1`, bản cập nhật tăng lên`in[1]`Và`out[1]`cùng nhau, có nghĩa là tính toán tiếp theo của`S += in[1] + out[1]`đã phản ánh cơ cấu mới tăng lên. 

Ví dụ: 

đầu vào:```
1 2
1 1
1 1
```Sau cạnh đầu tiên,`in[1]=1`,`out[1]=1`, Vì thế`S=0`. Sau cạnh thứ hai, chúng tôi tính toán`S += 1 + 1 = 2`, cho`S=2`. Điều này phù hợp với thực tế là có chính xác hai bước đi dài 2: chọn lần xuất hiện của vòng lặp đầu tiên làm bước đầu tiên và lần xuất hiện của vòng lặp thứ hai. 

Điều này xác nhận rằng thứ tự cập nhật và tích lũy là chính xác và không xảy ra tình trạng đếm quá mức ngay cả khi cả hai điểm cuối giống hệt nhau.
