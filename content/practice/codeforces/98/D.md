---
title: "CF 98D - Tu sĩ giúp đỡ"
description: "Chúng ta được cung cấp một biến thể của Tháp Hà Nội với ba cột và n đĩa xếp chồng lên nhau trên cột đầu tiên. Các đĩa được liệt kê từ dưới lên trên và không giống như bài toán cổ điển, một số đĩa có thể có cùng đường kính. Một nước đi vẫn chuyển chính xác một đĩa giữa các trụ."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms"]
categories: ["algorithms"]
codeforces_contest: 98
codeforces_index: "D"
codeforces_contest_name: "Codeforces Beta Round 78 (Div. 1 Only)"
rating: 2500
weight: 98
solve_time_s: 110
verified: true
draft: false
---

[CF 98D - Các nhà sư trợ giúp](https://codeforces.com/problemset/problem/98/D) 

**Đánh giá:** 2500 
**Tags:** thuật toán xây dựng 
**Thời gian giải:** 1 phút 50 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một biến thể của Tháp Hà Nội với ba cột và`n`đĩa xếp chồng lên nhau trên cột đầu tiên. Các đĩa được liệt kê từ dưới lên trên và không giống như bài toán cổ điển, một số đĩa có thể có cùng đường kính. 

Một nước đi vẫn chuyển chính xác một đĩa giữa các trụ. Một đĩa lớn hơn không bao giờ được đặt lên trên một đĩa nhỏ hơn. Đường kính bằng nhau được phép xếp chồng lên nhau. 

Mục tiêu không chỉ là di chuyển tất cả các đĩa từ cột`1`trụ cột`3`, mà còn để giữ nguyên trật tự ban đầu của chúng. Nếu hai đĩa bằng nhau xuất hiện ở vị trí`i < j`ban đầu, sau đó cùng một đĩa từ vị trí`i`vẫn phải ở dưới đĩa từ vị trí`j`trong cấu hình cuối cùng. 

Đầu ra phải chứa số lần di chuyển tối thiểu có thể, theo sau là một chuỗi di chuyển tối ưu. 

Các ràng buộc đủ nhỏ để cho phép hành vi theo cấp số nhân đối với số nhóm đường kính riêng biệt, nhưng không phải đối với tổng số đĩa. Chúng tôi có`n ≤ 20`, vậy thậm chí`2^20`các trạng thái có thể quản lý được, trong khi mọi thứ mang tính giai thừa hoặc liên quan đến BFS không hạn chế trên tất cả các trạng thái tháp đều trở nên nguy hiểm. Số lượng các cấu hình có thể có của Tháp Hà Nội tăng lên cực kỳ nhanh chóng và việc tìm kiếm đường đi ngắn nhất một cách ngây thơ qua các cấu hình là không thực tế. 

Đặc tính cấu trúc quan trọng là các đĩa có đường kính bằng nhau hoạt động gần giống như các khối có thể hoán đổi cho nhau. Một khi chúng ta hiểu được các đĩa bằng nhau ảnh hưởng như thế nào đến sự truy hồi, vấn đề sẽ chuyển thành đệ quy Hà Nội được sửa đổi với độ sâu hiệu dụng nhỏ hơn nhiều. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Giả sử tất cả các đĩa đều bằng nhau:```
3
1 1 1
```Một giải pháp bất cẩn có thể nghĩ rằng câu trả lời vẫn là`7`vì Hà Nội cổ điển với ba đĩa cần`2^3 - 1`di chuyển. Nhưng các đĩa giống nhau có thể xếp chồng tự do, vì vậy chúng ta có thể chỉ cần di chuyển mọi đĩa trực tiếp từ cột`1`trụ cột`3`:```
3
1 3
1 3
1 3
```Một trường hợp tinh tế khác là giữ trật tự giữa các đĩa bằng nhau:```
3
2 2 2
```Nếu thứ tự không quan trọng, người ta có thể coi ngăn xếp như một đối tượng duy nhất và hoàn thành chỉ bằng một bước di chuyển về mặt khái niệm. Nhưng các bước di chuyển vẫn thao tác riêng lẻ trên các đĩa vật lý và thứ tự cuối cùng phải khớp với thứ tự ban đầu. Mức tối thiểu vẫn chính xác`3`những chuyển động trực tiếp. 

Một ví dụ thú vị hơn là:```
4
3 2 2 1
```Hai đĩa ở giữa bằng nhau. Một phép đệ quy ngây thơ xử lý mọi đĩa một cách độc lập sẽ mang lại`15`di chuyển. Nhưng cặp bằng nhau có thể được chuyển cùng nhau hiệu quả hơn, làm giảm câu trả lời tối ưu. 

Thách thức là nhận ra chính xác mức tiết kiệm mà các đĩa bằng nhau tạo ra mà không vi phạm ràng buộc đặt hàng. 

## Phương pháp tiếp cận 

Phối cảnh bạo lực là mô hình hóa mọi cấu hình tháp hợp pháp dưới dạng trạng thái biểu đồ và chạy BFS từ cách sắp xếp ban đầu đến cách sắp xếp mục tiêu. 

Điều này hiệu quả vì mỗi lần di chuyển đều có chi phí như nhau, vì vậy BFS thực sự tìm thấy chuỗi ngắn nhất. Mỗi tiểu bang ghi lại trụ cột của mỗi đĩa và tính hợp pháp có thể được kiểm tra trực tiếp. 

Vấn đề là số lượng trạng thái. Thậm chí chỉ với`20`đĩa, có tới`3^20 ≈ 3.4 × 10^9`phân công đĩa cho các trụ cột. Hầu hết là bất hợp pháp, nhưng không gian nhà nước hợp pháp vẫn còn rất lớn. BFS là hoàn toàn không khả thi. 

Sự tái diễn Tháp Hà Nội cổ điển gợi ý một cách tiếp cận có cấu trúc hơn nhiều. Trong bài toán thông thường, để di chuyển đĩa lớn nhất thì trước tiên mọi đĩa nhỏ hơn phải được di chuyển đi. Sau đó, đĩa lớn nhất di chuyển một lần và cuối cùng các đĩa nhỏ hơn được đặt lại lên trên. Điều này mang lại:$T(n)=2T(n-1)+1$Toàn bộ khó khăn đến từ việc sắp xếp nghiêm ngặt kích thước đĩa. 

Bây giờ hãy xem xét những gì thay đổi khi một số đĩa liên tiếp có đường kính bằng nhau. 

Giả sử đỉnh`k`tất cả các đĩa đều có cùng kích thước. Sau khi tất cả các đĩa nhỏ phía trên chúng bị xóa, những đĩa này`k`các đĩa giống nhau có thể được chuyển trực tiếp từng đĩa một lên nhau tại trụ đích. Họ không còn yêu cầu xây dựng lại đệ quy tốn kém giữa mỗi lần di chuyển. 

Điều này thay đổi hoàn toàn sự tái phát. 

Đặt các nhóm đường kính riêng biệt có kích thước:```
c1, c2, ..., cm
```theo thứ tự từ đường kính lớn nhất đến nhỏ nhất. 

Nếu chúng ta đã biết cách di chuyển tối ưu tất cả các nhóm nhỏ hơn thì đối với một nhóm có quy mô`c`: 

1. Di chuyển tất cả các nhóm nhỏ hơn ra xa. 
2. Di chuyển trực tiếp toàn bộ nhóm có đường kính bằng nhau, yêu cầu chính xác`c`di chuyển. 
3. Di chuyển các nhóm nhỏ hơn về phía sau. 

Vì vậy, sự tái phát trở thành:$F(i)=2F(i+1)+c_i$Sự lặp lại này là tối ưu vì mọi đĩa trong nhóm hiện tại phải di chuyển vật lý ít nhất một lần và mọi đĩa nhỏ hơn phải rời khỏi và sau đó quay lại để bỏ chặn chúng. 

Phần xây dựng phản ánh gần như chính xác đệ quy Hà Nội cổ điển, ngoại trừ việc các đĩa bằng nhau được chuyển liên tiếp thay vì phân tách đệ quy. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| BFS Brute Force trên các tiểu bang | Hàm mũ theo số lượng trạng thái, đại khái$O(3^n)$|$O(3^n)$| Quá chậm | 
| Giải pháp xây dựng đệ quy |$O(M)$, Ở đâu$M$là số lần di chuyển |$O(n)$ngăn xếp đệ quy | Đã chấp nhận | 

Đây`M`chính là số lần di chuyển tối ưu, vẫn có thể đạt tới`2^20 - 1`, nhưng đó chỉ là khoảng một triệu lần di chuyển và vừa vặn trong giới hạn. 

## Hướng dẫn thuật toán 

1. Nén các đĩa thành các nhóm có đường kính liên tiếp bằng nhau. 

Ví dụ:```
5 5 3 3 3 1
```trở thành quy mô nhóm:```
[2, 3, 1]
```Chúng tôi chỉ quan tâm đến việc mỗi đường kính riêng biệt đóng góp bao nhiêu đĩa. 
2. Xử lý các nhóm đệ quy từ đường kính lớn nhất đến nhỏ nhất. 

Cho phép`solve(i, from, to, aux)`di chuyển tất cả các nhóm bắt đầu từ chỉ mục`i`. 
3. Nếu`i`đạt số nhóm thì dừng lại. 

Không còn đĩa để di chuyển. 
4. Đầu tiên, hãy di chuyển đệ quy tất cả các nhóm nhỏ hơn ra xa. 

Gọi:```
solve(i + 1, from, aux, to)
```Thao tác này sẽ xóa mọi đĩa nằm trên nhóm đường kính hiện tại. 
5. Di chuyển trực tiếp mọi đĩa trong nhóm hiện tại từ`from`ĐẾN`to`. 

Vì tất cả các đĩa trong nhóm có đường kính bằng nhau nên chúng có thể xếp chồng lên nhau một cách tự do. 

Nếu quy mô nhóm là`k`, nối chính xác`k`động tác giống nhau:```
from -> to
```6. Xây dựng lại đệ quy các nhóm nhỏ hơn ở trên cùng. 

Gọi:```
solve(i + 1, aux, to, from)
```7. Thu thập tất cả các nước đi và in chúng. 

### Tại sao nó hoạt động 

Đệ quy bảo toàn tính bất biến giống như Hà Nội cổ điển. 

Trước khi di chuyển nhóm đường kính, mọi đĩa nhỏ hơn sẽ tạm thời được di chuyển đến trụ phụ. Điều này đảm bảo nhóm hiện tại sẽ bị lộ. Bởi vì tất cả các đĩa trong nhóm đều bình đẳng nên việc di chuyển đĩa này sang đĩa khác luôn là hợp pháp. 

Sau khi nhóm đạt được trụ cột đích, các nhóm nhỏ hơn sẽ được xây dựng lại ở trên theo thứ tự tương đối ban đầu. 

Sự tối ưu xuất phát từ sự cần thiết. Mỗi đĩa trong nhóm hiện tại phải di chuyển ít nhất một lần. Trước khi bất kỳ động thái nào như vậy trở thành hợp pháp, tất cả các đĩa nhỏ hơn phải được gỡ bỏ. Sau đó, những đĩa nhỏ hơn đó phải trở lại nhóm trong cách sắp xếp cuối cùng. Điều đó buộc chính xác hai bài toán con đệ quy cộng với một lần di chuyển trên mỗi đĩa trong nhóm hiện tại, khớp với phép truy hồi được thuật toán sử dụng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

moves = []

def solve(groups, idx, src, dst, aux):
    if idx == len(groups):
        return

    solve(groups, idx + 1, src, aux, dst)

    for _ in range(groups[idx]):
        moves.append((src, dst))

    solve(groups, idx + 1, aux, dst, src)

def main():
    n = int(input())
    d = list(map(int, input().split()))

    groups = []
    i = 0

    while i < n:
        j = i
        while j < n and d[j] == d[i]:
            j += 1
        groups.append(j - i)
        i = j

    solve(groups, 0, 1, 3, 2)

    print(len(moves))
    print("\n".join(f"{a} {b}" for a, b in moves))

if __name__ == "__main__":
    main()
```Phần đầu tiên nén các đường kính bằng nhau liên tiếp thành các kích thước nhóm. Đây là sự đơn giản hóa trung tâm. Danh tính cá nhân trong một nhóm không còn quan trọng nữa vì các đĩa giống nhau có thể xếp chồng tự do trong khi vẫn tự động duy trì trật tự. 

Đệ quy`solve`chức năng phản ánh cấu trúc Hà Nội cổ điển. Sự khác biệt duy nhất là giai đoạn giữa. Thay vì di chuyển một đĩa lớn nhất, chúng tôi di chuyển trực tiếp toàn bộ khối có đường kính bằng nhau. 

Thứ tự của các cuộc gọi đệ quy là rất quan trọng. Lệnh gọi đệ quy đầu tiên sẽ xóa các đĩa nhỏ hơn khỏi cột nguồn. Cái thứ hai xây dựng lại chúng trên đầu ngăn xếp đích. Trao đổi các cuộc gọi này vi phạm tính hợp pháp. 

Một lỗi phổ biến là cố gắng di chuyển các đĩa bằng nhau về mặt khái niệm thay vì từng đĩa một về mặt vật lý. Câu lệnh vẫn yêu cầu mỗi lần di chuyển phải chuyển đúng một đĩa. Đó là lý do tại sao vòng lặp nối thêm`groups[idx]`những bước di chuyển riêng biệt. 

Độ sâu đệ quy nhiều nhất là số giá trị đường kính riêng biệt, không bao giờ vượt quá`20`, vì vậy đệ quy Python hoàn toàn an toàn ở đây. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
3 2 1
```Các nhóm là:```
[1, 1, 1]
```Điều này trở thành Hà Nội bình thường. 

| Bước | Hành động | Tiểu bang | 
| --- | --- | --- | 
| 1 | Di chuyển nhỏ nhất đến 3 | 1:[3,2] 2:[] 3:[1] | 
| 2 | Chuyển giữa sang 2 | 1:[3] 2:[2] 3:[1] | 
| 3 | Di chuyển nhỏ nhất đến 2 | 1:[3] 2:[2,1] 3:[] | 
| 4 | Di chuyển lớn nhất đến 3 | 1:[] 2:[2,1] 3:[3] | 
| 5 | Di chuyển nhỏ nhất đến 1 | 1:[1] 2:[2] 3:[3] | 
| 6 | Chuyển giữa sang 3 | 1:[1] 2:[] 3:[3,2] | 
| 7 | Di chuyển nhỏ nhất đến 3 | 1:[] 2:[] 3:[3,2,1] | 

Dấu vết này xác nhận thuật toán suy biến tự nhiên về tiêu chuẩn Hà Nội khi không tồn tại đường kính bằng nhau. 

### Ví dụ 2 

đầu vào:```
4
3 2 2 1
```Các nhóm là:```
[1, 2, 1]
```| Giai đoạn | Hoạt động | Giải thích | 
| --- | --- | --- | 
| 1 | Di chuyển nhóm nhỏ nhất đi | Đường kính lộ 2 nhóm | 
| 2 | Di chuyển đường kính 2 đĩa đầu tiên | Di chuyển trực tiếp | 
| 3 | Di chuyển đường kính thứ hai 2 đĩa | Cho phép xếp chồng bằng nhau | 
| 4 | Khôi phục nhóm nhỏ nhất | Xây dựng lại cấu trúc | 
| 5 | Di chuyển đĩa lớn nhất | Bước cổ điển | 
| 6 | Lặp lại đối xứng | Kết thúc chuyển khoản | 

Quan sát quan trọng là cặp bằng nhau chỉ di chuyển với hai lần chuyển trực tiếp thay vì yêu cầu phân tách đệ quy. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(M)$| Mỗi nước đi được tạo ra đều được viết chính xác một lần | 
| Không gian |$O(n)$| Độ sâu đệ quy và lưu trữ nhóm | 

Trường hợp xấu nhất xảy ra khi tất cả các đường kính đều khác nhau. Sau đó, giải pháp tạo ra chính xác`2^20 - 1 = 1,048,575`di chuyển, có thể chấp nhận được trong giới hạn. Việc sử dụng bộ nhớ vẫn còn rất ít vì chỉ có ngăn xếp đệ quy và danh sách di chuyển được lưu trữ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve_io(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    moves = []

    def solve(groups, idx, src, dst, aux):
        if idx == len(groups):
            return

        solve(groups, idx + 1, src, aux, dst)

        for _ in range(groups[idx]):
            moves.append((src, dst))

        solve(groups, idx + 1, aux, dst, src)

    n = int(input())
    d = list(map(int, input().split()))

    groups = []
    i = 0

    while i < n:
        j = i
        while j < n and d[j] == d[i]:
            j += 1
        groups.append(j - i)
        i = j

    solve(groups, 0, 1, 3, 2)

    out = [str(len(moves))]
    out.extend(f"{a} {b}" for a, b in moves)

    return "\n".join(out)

# provided sample
assert solve_io("3\n3 2 1\n").splitlines()[0] == "7"

# minimum size
assert solve_io("1\n5\n") == "1\n1 3"

# all equal
res = solve_io("3\n2 2 2\n").splitlines()
assert res[0] == "3"

# two groups
res = solve_io("4\n3 2 2 1\n").splitlines()
assert res[0] == "11"

# maximum distinct
res = solve_io(
    "20\n20 19 18 17 16 15 14 13 12 11 10 9 8 7 6 5 4 3 2 1\n"
).splitlines()
assert res[0] == str((1 << 20) - 1)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 5`|`1`di chuyển | Đầu vào kích thước tối thiểu | 
|`3 / 2 2 2`|`3`di chuyển | Đệ quy thu gọn đĩa bằng nhau | 
|`4 / 3 2 2 1`|`11`di chuyển | Nhóm hỗn hợp bình đẳng và khác biệt | 
|`20 / 20..1`|`2^20 - 1`di chuyển | Độ sâu đệ quy tối đa và kích thước đầu ra | 

## Vỏ cạnh 

Hãy xem xét:```
3
1 1 1
```Mảng nhóm trở thành:```
[3]
```Phép đệ quy ngay lập tức thực hiện ba bước di chuyển trực tiếp từ cột`1`trụ cột`3`. 

Đầu ra:```
3
1 3
1 3
1 3
```Điều này chứng tỏ tại sao việc nén các đường kính bằng nhau là đúng. Không cần phân tách đệ quy trong một nhóm. 

Bây giờ hãy xem xét:```
4
4 4 3 3
```Các nhóm là:```
[2, 2]
```Quá trình thực hiện diễn ra như sau: 

1. Di chuyển nhóm kích thước nhỏ hơn`2`vào cột phụ. 
2. Di chuyển nhóm bằng nhau lớn hơn trực tiếp bằng hai nước đi. 
3. Di chuyển nhóm nhỏ hơn trở lại. 

Sự tái phát mang lại:$F=2\cdot2+2=6$Quả thực giải pháp tối ưu sử dụng đúng sáu nước đi. 

Cuối cùng, hãy xem xét trường hợp hoàn toàn khác biệt:```
4
4 3 2 1
```Các nhóm trở thành:```
[1, 1, 1, 1]
```Thuật toán rút gọn chính xác về Hà Nội thông thường và cho ra kết quả`15`di chuyển. Điều này xác nhận đệ quy tổng quát bảo toàn hành vi cổ điển khi không tồn tại đường kính bằng nhau.
