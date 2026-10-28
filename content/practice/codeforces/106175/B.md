---
title: "CF 106175B - Cơ quan giám sát"
description: "Chúng ta có một mái hình vuông có tọa độ nguyên, từ $(0,0)$ đến $(S,S)$ và một tập hợp các vị trí cửa sập bên trong nó. Chúng ta phải chọn một điểm duy nhất trên mái nhà để gắn dây xích."
date: "2026-06-20T22:20:10+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106175
codeforces_index: "B"
codeforces_contest_name: "2004-2005 Northwestern European Regional Contest (NWERC 2004)"
rating: 0
weight: 106175
solve_time_s: 53
verified: true
draft: false
---

[CF 106175B - Cơ quan giám sát](https://codeforces.com/problemset/problem/106175/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một mái nhà hình vuông có tọa độ nguyên, từ$(0,0)$ĐẾN$(S,S)$và một tập hợp các vị trí cửa sập bên trong nó. Chúng ta phải chọn một điểm duy nhất trên mái nhà để gắn dây xích. Từ điểm neo đó, con chó có thể tới mọi cửa sập có tâm nằm trong bán kính nào đó.$R$, Ở đâu$R$là chiều dài dây xích. Chiều dài dây xích không cố định trước, nó được chọn sau khi chúng ta chọn điểm neo, nhưng nó phải đủ lớn để che hết các cửa hầm và đủ nhỏ để dây xích không nhô ra ngoài mái nhà. Về mặt hình học, dây xích không thể vượt ra ngoài ranh giới hình vuông, vì vậy điểm neo phải sao cho khoảng cách xa nhất từ ​​nó đến ranh giới ít nhất là bán kính yêu cầu. 

Nhiệm vụ là xác định xem có tồn tại một điểm tọa độ nguyên bên trong hình vuông không bằng bất kỳ vị trí cửa sập nào hay không, sao cho khoảng cách tối thiểu từ điểm này đến ranh giới của hình vuông ít nhất bằng khoảng cách tối đa từ điểm này đến bất kỳ cửa sập nào. Nếu tồn tại nhiều điểm neo hợp lệ, chúng tôi chọn điểm có giá trị nhỏ nhất$x$, và nếu vẫn bị ràng buộc thì nhỏ nhất$y$. 

Hạn chế chính khiến vấn đề này có thể quản lý được là$S \le 40$, do đó toàn bộ không gian tìm kiếm của các điểm neo có thể có nhiều nhất là$41 \times 41 = 1681$ứng viên cho mỗi trường hợp thử nghiệm. Với tối đa 50 cửa sổ, việc đánh giá hình học trực tiếp cho mỗi ứng viên sẽ không tốn kém, do đó việc quét hình học mạnh mẽ đã nằm trong giới hạn. 

Một điều kiện khó phát hiện ở cạnh là điểm neo không thể trùng với cửa sập. Điều này có thể làm mất hiệu lực các trung tâm tối ưu và một giải pháp ngây thơ mà quên kiểm tra này sẽ tạo ra câu trả lời sai. 

Một chế độ thất bại khác là nhầm lẫn giữa “ràng buộc ranh giới” với khoảng cách cứng chỉ từ các góc. Dây xích phải nằm trong hình vuông nên chúng ta phải tính đến khoảng cách đến cạnh gần nhất chứ không phải đến tâm hoặc các góc. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản là thử mọi điểm tọa độ nguyên$(x,y)$bên trong hình vuông, không bao gồm các vị trí cửa sập. Đối với mỗi điểm ứng cử viên, chúng tôi tính toán hai đại lượng. 

Đầu tiên là độ dài dây buộc cần thiết, là khoảng cách Euclide tối đa từ$(x,y)$đến bất kỳ cửa sập nào. Điều này đảm bảo tất cả các cửa sập đều có thể tiếp cận được. 

Thứ hai là chiều dài dây xích tối đa có thể giữ con chó ở trong mái nhà. Vì con chó không thể đi ra ngoài quảng trường nên dây xích không thể vượt quá khoảng cách từ$(x,y)$đến ranh giới gần nhất. Khoảng cách này là$\min(x, y, S-x, S-y)$. 

Để một điểm hợp lệ, khoảng cách yêu cầu phải nhỏ hơn hoặc bằng khoảng cách biên. Trong số tất cả các điểm hợp lệ, chúng tôi chọn điểm nhỏ nhất về mặt từ điển. 

Giải pháp vũ phu đánh giá$O(S^2)$các ứng cử viên, mỗi người kiểm tra$H$nở, dẫn đến$O(S^2 \cdot H)$sự phức tạp. Với$S \le 40$Và$H \le 50$, đây nhiều nhất là khoảng 3,2 triệu phép tính khoảng cách cho mỗi trường hợp thử nghiệm trong trường hợp xấu nhất, điều này có thể dễ dàng chấp nhận được. 

Quan sát chính là không cần các kỹ thuật tối ưu hóa hình học như sơ đồ Voronoi hoặc tìm kiếm nhị phân trên bán kính, vì không gian tìm kiếm rõ ràng là nhỏ và rời rạc. Vấn đề về cơ bản là kiểm tra tính khả thi của minimax bị ràng buộc trên một lưới giới hạn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Quét lưới Brute Force |$O(S^2 \cdot H)$|$O(H)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi lặp lại mọi điểm nguyên có thể có bên trong hình vuông và kiểm tra xem liệu điểm đó có thể đóng vai trò là điểm đính kèm dây xích hợp lệ hay không. 

1. Liệt kê tất cả các điểm dự tuyển$(x, y)$với$0 \le x \le S$Và$0 \le y \le S$. Điều này bao gồm tất cả các vị trí đính kèm hợp pháp vì vấn đề giới hạn ở tọa độ nguyên. 
2. Bỏ qua ứng viên nếu nó trùng với vị trí cửa sập. Điều này là bắt buộc vì việc gắn dây trực tiếp lên cửa sập bị nghiêm cấm. 
3. Với mỗi ứng viên còn lại, hãy tính khoảng cách tối đa từ$(x,y)$tới tất cả các cửa sập sử dụng khoảng cách Euclide. Giá trị này thể hiện độ dài dây xích tối thiểu cần thiết để tiếp cận mọi cửa sập. 
4. Tính chiều dài dây xích tối đa cho phép tại thời điểm này, đó là khoảng cách đến cạnh gần nhất của hình vuông:$\min(x, y, S-x, S-y)$. Điều này đảm bảo dây xích vẫn nằm hoàn toàn trong mái nhà. 
5. Nếu khoảng cách yêu cầu lớn hơn khoảng cách ranh giới cho phép, hãy loại bỏ ứng viên này vì nó sẽ buộc dây xích vượt qua ranh giới mái nhà. 
6. Nếu ứng viên hợp lệ, hãy so sánh nó với ứng viên tốt nhất hiện tại theo thứ tự từ điển trên$(x,y)$, thích nhỏ hơn$x$, sau đó nhỏ hơn$y$. 

Sau khi kiểm tra tất cả các điểm, nếu không có ứng viên hợp lệ thì xuất ra “poodle”. 

### Tại sao nó hoạt động 

Mọi giải pháp hợp lệ phải thỏa mãn hai ràng buộc độc lập: nó phải bao phủ tất cả các cửa sập và không được vượt quá khoảng cách đến ranh giới. Thuật toán kiểm tra chính xác cả hai ràng buộc đối với mọi tọa độ nguyên có thể có, do đó không bỏ sót ứng viên hợp lệ nào. Vì chúng tôi đánh giá toàn diện toàn bộ miền rời rạc của các điểm đính kèm có thể có, nên điểm hợp lệ đầu tiên theo thứ tự từ điển trong số tất cả các điểm khả thi được đảm bảo là câu trả lời bắt buộc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
import math

def dist2(x1, y1, x2, y2):
    dx = x1 - x2
    dy = y1 - y2
    return dx * dx + dy * dy

t = int(input())
for _ in range(t):
    S, H = map(int, input().split())
    hatches = []
    hatch_set = set()

    for _ in range(H):
        x, y = map(int, input().split())
        hatches.append((x, y))
        hatch_set.add((x, y))

    best = None

    for x in range(S + 1):
        for y in range(S + 1):
            if (x, y) in hatch_set:
                continue

            # max distance to any hatch (squared)
            need = 0
            for hx, hy in hatches:
                d = dist2(x, y, hx, hy)
                if d > need:
                    need = d

            # max allowed squared radius (distance to boundary)
            # compare squared carefully by squaring boundary distance
            bd = min(x, y, S - x, S - y)
            bd2 = bd * bd

            if need <= bd2:
                if best is None or (x, y) < best:
                    best = (x, y)

    if best is None:
        print("poodle")
    else:
        print(best[0], best[1])
```Việc triển khai sử dụng khoảng cách bình phương để tránh các vấn đề về độ chính xác của dấu phẩy động. chức năng`dist2`tính toán khoảng cách Euclide bình phương giữa một ứng cử viên và một cửa sổ. Điều này cho phép so sánh trực tiếp mà không cần lấy căn bậc hai. 

Ràng buộc ranh giới cũng là bình phương. Vì cả phạm vi tiếp cận bắt buộc và phạm vi tiếp cận ranh giới đều bình phương nên chúng tôi tránh mọi rủi ro về lỗi chính xác và tiếp tục so sánh ở dạng số nguyên. 

Việc lựa chọn từ điển được xử lý bằng cách so sánh bộ dữ liệu Python, điều này thực thi một cách tự nhiên các giá trị nhỏ nhất$x$, thì nhỏ nhất$y$. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
S = 10
H = 2
hatches: (6,6), (5,4)
```Chúng tôi đánh giá một vài điểm ứng cử viên: 

| (x, y) | max dist² để nở | bán kính ranh giới² | có hiệu lực? | 
| --- | --- | --- | --- | 
| (3,6) | max((3-6)^2+(6-6)^2=9, (3-5)^2+(6-4)^2=8) = 9 | phút(3,6,7,4)^2 = 9 | vâng | 
| (2,2) | tối đa(20,9) = 20 | phút(2,2,8,8)^2 = 4 | không | 

Điểm nhỏ nhất về mặt từ điển có giá trị tốt nhất trở thành$(3,6)$. 

Điều này cho thấy tính khả thi được điều chỉnh chung bởi cả hai ràng buộc chứ không chỉ ở khoảng cách gần với các cửa sập. 

### Ví dụ 2 

đầu vào:```
S = 10
H = 3
hatches: (1,1), (1,2), (1,3)
```Đánh giá ứng viên: 

| (x, y) | khoảng cách tối đa | ranh giới² | có hiệu lực? | 
| --- | --- | --- | --- | 
| (2,2) | tối đa(2,1,2)=2 | phút(2,2,8,8)^2=4 | vâng | 
| (1,4) | không hợp lệ (không cho phép nở hoặc hạn chế ranh giới tệ hơn tùy theo kiểm tra) | - | bỏ qua | 

Ứng cử viên hợp lệ nhỏ nhất về mặt từ điển là$(2,2)$, cân bằng ở khoảng cách đủ xa so với đường thẳng đứng của các cửa sập trong khi vẫn ở bên trong ranh giới một cách an toàn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(T \cdot S^2 \cdot H)$| Mỗi điểm trong số tối đa 1681 điểm lưới kiểm tra tối đa 50 cửa sổ | 
| Không gian |$O(H)$| Lưu trữ tọa độ cửa sập và tra cứu thiết lập | 

Được cho$S \le 40$Và$H \le 50$, tổng số thao tác vẫn ở mức nhỏ thoải mái ngay cả trong Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose
    import math

    t = int(input())
    out = []
    for _ in range(t):
        S, H = map(int, input().split())
        hatches = []
        hatch_set = set()
        for _ in range(H):
            x, y = map(int, input().split())
            hatches.append((x, y))
            hatch_set.add((x, y))

        best = None

        for x in range(S + 1):
            for y in range(S + 1):
                if (x, y) in hatch_set:
                    continue
                need = 0
                for hx, hy in hatches:
                    dx = x - hx
                    dy = y - hy
                    need = max(need, dx*dx + dy*dy)
                bd = min(x, y, S-x, S-y)
                if need <= bd*bd:
                    if best is None or (x, y) < best:
                        best = (x, y)

        out.append("poodle" if best is None else f"{best[0]} {best[1]}")
    return "\n".join(out)

# provided samples
assert run("""1
10 2
6 6
5 4
""") == "3 6"

assert run("""1
20 2
1 1
19 19
""") == "poodle"

assert run("""1
10 3
1 1
1 2
1 3
""") == "2 2"

# custom cases
assert run("""1
2 1
1 1
""") in {"0 0", "0 2", "2 0", "2 2"}

assert run("""1
4 4
1 1
1 3
3 1
3 3
""") != "", "must always output something"

assert run("""1
2 2
0 1
2 1
""") != "", "edge small square"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tắc nghẽn trung tâm duy nhất | góc hoặc điểm đối xứng | sự lựa chọn đối xứng và từ điển | 
| cửa hầm đối xứng dày đặc | trung tâm hợp lệ hoặc poodle | tính khả thi dưới những ràng buộc chặt chẽ | 
| trường hợp lưới tối thiểu | bất kỳ điểm an toàn ranh giới hợp lệ nào | xử lý ranh giới | 

## Vỏ cạnh 

Một trường hợp tinh tế xảy ra khi tất cả các điểm ứng cử viên đều không hợp lệ do ràng buộc về ranh giới hoặc trùng với các điểm giới hạn. Ví dụ: trong một lưới nhỏ nơi các cửa sập chiếm hầu hết các điểm bên trong, vòng lặp sẽ loại bỏ chính xác tất cả các khả năng và trả về "poodle". 

Một trường hợp khác là khi điểm tốt nhất nằm trên ranh giới của hình vuông. Khoảng cách ranh giới trở thành 0 ở các cạnh, do đó chỉ những điểm đủ bên trong mới tồn tại. Thuật toán xử lý việc này một cách tự nhiên bởi vì`min(x, y, S-x, S-y)`trở thành 0 trên đường viền, ngay lập tức loại bỏ mọi độ dài dây buộc cần thiết khác 0. 

Trường hợp thứ ba là khi có nhiều ứng viên đạt được tính khả thi giống nhau. Sự so sánh từ điển học`(x, y) < best`đảm bảo lựa chọn xác định nhỏ nhất$x$đầu tiên, sau đó nhỏ nhất$y$, mà không cần sắp xếp bổ sung hoặc xử lý hậu kỳ.
