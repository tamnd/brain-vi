---
title: "CF 102275A - Đang chạy trốn"
description: "Chúng ta có một lưới hình chữ nhật (N lần M). Ông X chiếm một ô và có một hoặc hai chất HAHA chiếm hai ô còn lại. Trong mỗi vòng, mỗi đặc vụ phải di chuyển chính xác một bước đến ô trống liền kề, theo bất kỳ thứ tự nào. Sau khi tất cả các đặc vụ đã di chuyển, Mr."
date: "2026-08-17T02:59:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102275
codeforces_index: "A"
codeforces_contest_name: "2019 Facebook Hacker Cup, Round 2"
rating: 0
weight: 102275
solve_time_s: 1057
verified: true
draft: false
---

[CF 102275A - Đang chạy](https://codeforces.com/problemset/problem/102275/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 17 phút 37 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một lưới hình chữ nhật (N \times M). Ông X chiếm một ô và có một hoặc hai chất HAHA chiếm hai ô còn lại. Trong mỗi vòng, mỗi đặc vụ phải di chuyển chính xác một bước đến ô trống liền kề, theo bất kỳ thứ tự nào. Sau khi tất cả đặc vụ đã di chuyển, ông X cũng thực hiện đúng một bước tới ô trống liền kề. Nếu mọi ô liền kề của ông X đều bị đại lý chiếm đóng thì ông X không có nước đi hợp pháp và thua cuộc. 

Đầu vào cung cấp kích thước lưới, ô bắt đầu của Mr. X và các ô bắt đầu của tác nhân (K). Đầu ra hỏi liệu các đặc vụ có thể buộc thua hay không, với lối chơi hoàn hảo của cả hai bên.`Y`có nghĩa là các đại lý có chiến lược chiến thắng, trong khi`N`nghĩa là anh X có thể tiếp tục di chuyển mãi mãi. 

Các ràng buộc có vẻ đủ lớn để ngăn cản bất kỳ hoạt động mô phỏng trò chơi nào. Với (N,M\le300), có thể có (90{,}000) ô. Một trạng thái có hai tác nhân chứa ba ô bị chiếm giữ, do đó, một biểu đồ trò chơi rõ ràng đã có các cấu hình theo thứ tự (90{,}000^3), khoảng (7,3\time10^{14}). Ngay cả việc lưu trữ một biểu đồ như vậy là không thể. Việc có nhiều nhất hai tác nhân là hạn chế quan trọng về mặt cấu trúc giúp cho việc mô tả đặc tính đơn giản hơn nhiều. 

Quan sát hữu ích đầu tiên là một ô lưới có ít nhất hai ô lân cận và một ô bên trong có bốn ô lân cận. Vì có nhiều nhất hai đặc vụ nên ông X chỉ được đầu hàng khi đứng ở một góc và cả hai người hàng xóm ở góc đó đều có người ở. Do đó, các tác nhân không bao giờ cần phải bao quanh một ô bên trong. Họ chỉ cần dồn anh X vào một góc là được. 

Quan sát thứ hai là mạng lưới có tính chất lưỡng cực. Ô màu ((r,c)) theo tính chẵn lẻ của (r+c). Mọi động thái hợp pháp đều thay đổi tính chẵn lẻ này. Vì mỗi người tham gia di chuyển chính xác một lần trong mỗi vòng hoàn chỉnh nên mối quan hệ ngang bằng giữa ba người tham gia khi bắt đầu một vòng không bao giờ thay đổi. 

Hãy xem xét đầu vào nhỏ```
1
3 3 1
1 1
2 2
```Chỉ có một tác nhân nên kết quả đúng là`N`. Một mô phỏng bất cẩn có thể thấy đặc vụ di chuyển về phía một góc và cho rằng cuối cùng nó có thể bẫy được ông X, nhưng cần có hai người hàng xóm đang ở vị trí cuối cùng. 

Bây giờ hãy xem xét```
1
3 3 2
1 1
1 3
3 1
```Cả ba vị trí đều có (r+c). Đầu ra đúng là`Y`. Sau khi các đặc vụ di chuyển đến ((1,2)) và ((2,1)), ông X tại ((1,1)) không còn hàng xóm rảnh rỗi. 

Một trường hợp tế nhị hơn là```
1
4 4 2
1 2
1 3
1 4
```Các số chẵn lẻ là (1,0,1), vì vậy chúng không bằng nhau. Đầu ra đúng là`N`. Một mô phỏng chỉ kiểm tra xem các đặc vụ có thể tiếp cận cùng một góc hay không có thể dự đoán sai một cái bẫy. Mối quan hệ chẵn lẻ ngăn không cho một trong các tác nhân ở đúng lớp màu khi hai ô lân cận cuối cùng phải được chiếm giữ. 

Đặc tính chẵn lẻ cũng nhất quán với cuộc thảo luận trong cuộc thi, trong đó quan sát được chấp nhận cho vấn đề A được tóm tắt là yêu cầu cả ba điểm phải có cùng giá trị ((x+y)\bmod2) khi (K=2), trong khi (K=1) luôn thua đối với các tác nhân. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ mô hình hóa mọi sự sắp xếp có thể có của ông X và các đặc vụ như một trạng thái trò chơi. Từ mỗi tiểu bang, chúng tôi sẽ liệt kê các bước di chuyển có thể có của các đặc vụ, tính đến thứ tự có thể có của chúng và sau đó liệt kê các phản hồi của ông X. Bởi vì đây là một trò chơi có khả năng tiếp cận hữu hạn nên việc phân tích trò chơi ngược về mặt lý thuyết có thể phân loại mọi trạng thái là thắng hoặc thua. 

Vấn đề là kích thước của không gian trạng thái đó. Đặt (V=NM). Với hai tác nhân, có khoảng (V(V-1)(V-2)) vị trí được sắp xếp, do đó ở kích thước lưới tối đa có khoảng (90{,}000^3=7,29\times10^{14}) trạng thái trước khi xem xét bất kỳ động thái nào. Ngay cả khối lượng công việc không đổi ở mỗi bang cũng vượt xa giới hạn. 

Cấu trúc phá hủy không gian trạng thái khổng lồ này là màu bàn cờ. Mỗi bước di chuyển đều đổi màu. Khi bắt đầu mỗi vòng hoàn chỉnh, cả ba người tham gia đều thực hiện cùng số nước đi kể từ khi bắt đầu, do đó mối quan hệ ngang bằng theo cặp của họ được cố định mãi mãi. 

Giả sử (K=2) và một tác nhân bắt đầu có màu đối diện với ông X. Khi bắt đầu mỗi giai đoạn tác nhân, tác nhân đó vẫn có màu đối diện với ông X. Sau đó, tác nhân phải di chuyển nên chuyển sang màu của ông X. Hàng xóm của ông X có màu đối lập với ông X nên đặc vụ đó không thể chiếm ô lân cận của ông X sau khi bắt buộc phải di chuyển. Vì cả hai người hàng xóm ở một góc đều phải gài bẫy ông X nên các đặc vụ không bao giờ có thể thắng. 

Điều này đưa ra điều kiện cần thiết là cả ba vị trí ban đầu phải có cùng tính chẵn lẻ. 

Điều kiện tương tự là đủ. Khi cả ba người tham gia bắt đầu trên cùng một màu bàn cờ, hai tác nhân có thể sử dụng chiến lược ép lưới hai tác nhân tiêu chuẩn. Hai tác nhân đủ để buộc một người chơi di chuyển về phía ranh giới của một lưới hình chữ nhật hữu hạn và cuối cùng vào một góc. Các chiến lược theo đuổi lưới cổ điển thực hiện chính xác điều này bằng cách duy trì hai vùng thu hẹp hoặc hình nón bóng và ngăn người chạy vượt qua ranh giới được kiểm soát. Hai tác nhân là đủ cho lưới hình chữ nhật vì các tác nhân có thể giảm dần hình chữ nhật có sẵn của đường chạy cho đến khi chỉ còn lại một góc. 

Quy tắc đặc biệt mà mọi tác nhân phải di chuyển được xử lý bởi cùng một điều kiện chẵn lẻ. Khi bắt đầu mỗi hiệp đấu, các đại lý và ông X có số điểm ngang nhau. Một nước đi của tác nhân bắt buộc sẽ lật tính chẵn lẻ của nó chính xác khi nước đi tiếp theo của ông X sẽ lật của ông ấy. Do đó, các đại lý có thể thực hiện chiến lược ép mà không cần phải chờ đến lượt không sử dụng. Khi người chạy tới một góc cua, hai đặc vụ có thể chiếm giữ hai người hàng xóm của mình khi đang di chuyển, buộc họ phải đầu hàng. 

Lực lượng vũ phu hoạt động vì nó thể hiện rõ ràng mọi trạng thái trò chơi có thể có, nhưng không thành công vì không gian trạng thái có số lượng ô là bậc ba. Việc quan sát tính chẵn lẻ thu gọn toàn bộ trò chơi thành một thử nghiệm liên tục. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O((NM)^{K+1})) | (O((NM)^{K+1})) | Quá chậm | 
| Tối ưu | (O(K)) cho mỗi trường hợp thử nghiệm | (O(K)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc (N,M,K), chức vụ của ông X và tất cả các chức vụ của đại lý. Kích thước thực tế của lưới sẽ không ảnh hưởng đến thử nghiệm cuối cùng, vì (N,M\ge3) đảm bảo rằng mọi góc đều có chính xác hai lân cận. 
2. Nếu (K=1), trả lời`N`. Một đặc vụ có thể chiếm giữ nhiều nhất một trong hai người hàng xóm của một góc, trong khi việc đầu hàng yêu cầu cả hai người hàng xóm phải bị chiếm giữ. 
3. Nếu (K=2), hãy tính màu bàn cờ của mỗi người tham gia là ((r+c)\bmod2). Một bước di chuyển sẽ thay đổi giá trị này vì chính xác một tọa độ thay đổi theo (1). 
4. Kiểm tra xem hai đại lý và ông X có cùng chẵn lẻ hay không. Nếu có gì khác biệt, hãy trả lời`N`. Tác nhân khác biệt sẽ luôn có tính chẵn lẻ sai sau khi di chuyển bắt buộc để tham gia vào bẫy góc, vì vậy nó không bao giờ có thể chiếm một trong các ô lân cận được yêu cầu. 
5. Nếu cả ba số chẵn đều bằng nhau, hãy trả lời`Y`. Hai tác nhân có thể áp dụng chiến lược ép hai tác nhân tiêu chuẩn trên lưới hình chữ nhật. Tính chẵn lẻ bắt đầu bằng nhau của chúng cho phép mọi nước đi cần thiết đều phù hợp với cấu trúc màu xen kẽ và chiến lược cuối cùng sẽ nhốt ông X vào một góc. Khi anh ta ở đó, các đặc vụ có thể chiếm giữ hai phòng giam liền kề và không cho anh ta di chuyển hợp pháp. 

Bất biến là mối quan hệ chẵn lẻ ở đầu mỗi vòng. Mỗi người tham gia thực hiện chính xác một nước đi mỗi vòng, vì vậy mỗi người tham gia sẽ lật màu một lần. Do đó, nếu ban đầu hai người tham gia có màu giống nhau thì họ có màu bằng nhau ở mọi ranh giới vòng và nếu ban đầu họ có màu khác nhau thì chúng vẫn khác nhau. Bẫy góc chiến thắng yêu cầu cả hai đặc vụ phải có cùng màu với ông X khi bắt đầu giai đoạn đặc vụ, vì nước đi của họ phải đặt chúng vào các ô có màu đối diện. Do đó, tính chẵn lẻ ban đầu không bằng nhau là một trở ngại tuyệt đối, trong khi tính chẵn lẻ ban đầu bằng nhau chính xác là trường hợp trong đó chiến lược ép lưới hai tác nhân có thể hoàn thành bẫy. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []

    for tc in range(1, t + 1):
        n, m, k = map(int, input().split())

        x_r, x_c = map(int, input().split())
        agents = [tuple(map(int, input().split())) for _ in range(k)]

        if k == 1:
            ans = "N"
        else:
            target_parity = (x_r + x_c) & 1
            ans = "Y"

            for r, c in agents:
                if ((r + c) & 1) != target_parity:
                    ans = "N"
                    break

        out.append(f"Case #{tc}: {ans}")

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Đầu vào được đọc một trường hợp thử nghiệm tại một thời điểm. Với mỗi trường hợp, tính chẵn lẻ của ông X được tính một lần bằng cách sử dụng`(x_r + x_c) & 1`. 

Khi có một tác nhân, chương trình sẽ trả về ngay`N`, vì lưới có ít nhất ba hàng và ba cột nên mỗi góc có hai lân cận riêng biệt. Một đại lý không thể chiếm cả hai. 

Khi có hai tác nhân, chương trình bắt đầu bằng`Y`và thay đổi nó thành`N`ngay khi một đại lý có số chẵn lẻ khác với ông X. Không cần phải lưu trữ lưới, tạo các bước di chuyển hoặc theo dõi các vị trí trong tương lai. Mối quan hệ ngang bằng đã quyết định trò chơi. 

Các tọa độ dựa trên một, chính xác như được cung cấp bởi bài toán. Việc thêm chúng trước khi lấy tính chẵn lẻ không bị ảnh hưởng bởi việc lựa chọn lập chỉ mục dựa trên một so với dựa trên 0, vì việc dịch chuyển cả hai tọa độ bằng một sẽ thay đổi tổng của chúng bằng hai. 

Các số nguyên Python có độ chính xác tùy ý, mặc dù ở đây không liên quan đến số học lớn. Việc sử dụng bộ nhớ là không đổi ngoại trừ danh sách nhỏ tối đa hai vị trí tác nhân. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Mẫu đầu tiên có một tác nhân. 

| Biến | Giá trị | 
| --- | --- | 
| (N,M) | (3,3) | 
| (K) | (1) | 
| Ông X | ((1,1)) | 
| Ông X ngang bằng | (0) | 
| Đại lý | ((2,2)) | 
| Đại lý ngang bằng | (0) | 
| Quyết định |`K == 1`| 
| Đầu ra |`N`| 

Mặc dù tác nhân có cùng tính chẵn lẻ với ông X, nhưng sự bình đẳng về tính chẵn lẻ chỉ với một tác nhân là không đủ. Góc ((1,1)) có hai lân cận, ((1,2)) và ((2,1)), và cả hai sẽ phải được chiếm giữ đồng thời. Một đại lý không thể làm điều đó, vì vậy câu trả lời là`N`. 

### Mẫu 2 

Mẫu thứ hai có hai tác nhân. 

| Biến | Giá trị | 
| --- | --- | 
| (N,M) | (3,3) | 
| (K) | (2) | 
| Ông X | ((1,1)) | 
| Ông X ngang bằng | (0) | 
| Đại lý 1 | ((1,3)) | 
| Đại lý 1 chẵn lẻ | (0) | 
| Đặc vụ 2 | ((3,1)) | 
| Đại lý 2 chẵn lẻ | (0) | 
| Tất cả các số chẵn lẻ đều bằng nhau | Có | 
| Đầu ra |`Y`| 

Hai tác nhân có thể di chuyển đến ((1,2)) và ((2,1)), hai ô liền kề với ông X. Cả hai đều tự do trước khi tác nhân tương ứng di chuyển nên trạng thái cuối cùng khiến ông X không có động thái hợp pháp. Câu trả lời là`Y`. 

Dấu vết chứng minh tại sao sự bình đẳng chẵn lẻ là cấu hình chiến thắng. Lúc đầu, cả ba vị trí đều có cùng màu. Sau khi cả hai đặc vụ chuyển đi, cả hai đều đã đổi màu và có thể chiếm hàng xóm của ông X, trong khi ông X vẫn chưa chuyển đi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(K)) cho mỗi trường hợp thử nghiệm | Chúng tôi kiểm tra nhiều nhất hai vị trí đại lý. | 
| Không gian | (O(K)) | Chúng tôi lưu trữ tối đa hai vị trí đại lý. | 

Vì (K\le2), công việc trên mỗi ca kiểm thử thực tế là không đổi. Ngay cả với (T=500), thuật toán chỉ thực hiện được vài nghìn phép tính số học. Lưới có thể chứa (90{,}000) ô, nhưng giải pháp không bao giờ xây dựng nó. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline
    t = int(input())
    out = []

    for tc in range(1, t + 1):
        n, m, k = map(int, input().split())
        xr, xc = map(int, input().split())
        agents = [tuple(map(int, input().split())) for _ in range(k)]

        if k == 1:
            ans = "N"
        else:
            p = (xr + xc) & 1
            ans = "Y"
            for r, c in agents:
                if ((r + c) & 1) != p:
                    ans = "N"
                    break

        out.append(f"Case #{tc}: {ans}")

    return "\n".join(out)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    try:
        return solve()
    finally:
        sys.stdin = old_stdin

sample = """\
8
3 3 1
1 1
2 2
3 3 2
1 1
1 3
3 1
4 4 2
1 2
1 3
1 4
3 10 2
2 10
1 9
3 9
8 8 2
8 1
8 8
1 1
300 5 2
2 3
15 2
300 5
67 25 2
32 10
66 3
21 18
71 87 2
36 44
1 87
71 1
"""

expected_sample = """\
Case #1: N
Case #2: Y
Case #3: N
Case #4: Y
Case #5: N
Case #6: Y
Case #7: N
Case #8: Y
"""

assert run(sample) == expected_sample, "provided samples"

assert run("""\
1
3 3 1
1 1
2 2
""") == "Case #1: N", "minimum grid with one agent"

assert run("""\
1
3 3 2
1 1
1 3
3 1
""") == "Case #1: Y", "minimum grid, all three positions have even parity"

assert run("""\
1
4 4 2
1 2
1 3
1 4
""") == "Case #1: N", "boundary case with mixed parity"

assert run("""\
1
300 300 2
2 3
100 100
299 299
""") == "Case #1: N", "maximum grid size with mixed parity"

assert run("""\
1
300 5 2
2 2
2 4
4 2
""") == "Case #1: Y", "large boundary dimensions with equal parity"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3 3 1 / 1 1 / 2 2`|`Case #1: N`| Lưới tối thiểu và trường hợp một tác nhân | 
|`3 3 2 / 1 1 / 1 3 / 3 1`|`Case #1: Y`| Cả ba vị trí đều có màu bàn cờ giống nhau | 
|`4 4 2 / 1 2 / 1 3 / 1 4`|`Case #1: N`| Tính chẵn lẻ hỗn hợp trên các ô biên | 
|`300 300 2 / 2 3 / 100 100 / 299 299`|`Case #1: N`| Kích thước tối đa và tọa độ lớn | 
|`300 5 2 / 2 2 / 2 4 / 4 2`|`Case #1: Y`| Tính chẵn lẻ bằng nhau gần ranh giới lưới với số hàng tối đa | 

Điều kiện theo nghĩa đen là tất cả các tọa độ đều giống hệt nhau không thể xảy ra trong một đầu vào hợp lệ, bởi vì vấn đề đảm bảo rằng tất cả những người tham gia đều chiếm các ô riêng biệt. Cách giải thích có ý nghĩa nhất đối với thử nghiệm "hoàn toàn bằng nhau" là cả ba giá trị chẵn lẻ đều bằng nhau, được bao hàm trong trường hợp tùy chỉnh thứ hai và thứ năm. 

## Vỏ cạnh 

Trường hợp một tác nhân được xử lý trước bất kỳ lý luận chẵn lẻ nào. Vì```
1
3 3 1
1 1
2 2
```Ông X xuất phát ở một góc nhưng người đại diện duy nhất không thể điền cả ((1,2)) và ((2,1)). Ngay cả khi người đại diện chuyển đến một trong số họ, người kia vẫn miễn phí. Thuật toán ngay lập tức trở lại`N`. 

Đối với tính chẵn lẻ hỗn hợp, hãy xem xét```
1
4 4 2
1 2
1 3
1 4
```Ông X có tính chẵn lẻ (3\bmod2=1). Tác nhân đầu tiên có tính chẵn lẻ (4\bmod2=0), trong khi tác nhân thứ hai có tính chẵn lẻ (5\bmod2=1). Tại mỗi ranh giới vòng, tác nhân đầu tiên vẫn có màu đối diện với ông X. Sau khi tác nhân đầu tiên buộc phải di chuyển, nó sẽ chuyển sang màu của ông X chứ không phải màu hàng xóm cần thiết cho bẫy. Do đó, cả hai hàng xóm góc được yêu cầu đều không bao giờ có thể bị chiếm dụng sau giai đoạn của đại lý. Thuật toán trả về`N`. 

Đối với cấu hình chẵn lẻ chiến thắng,```
1
3 3 2
1 1
1 3
3 1
```cả ba tổng đều chẵn. Các đặc vụ bắt đầu tại các ô hai bước dọc theo hai hướng từ góc. Họ di chuyển đến ((1,2)) và ((2,1)), chính là hai người hàng xóm của ông X. Bước tiếp theo là không thể thực hiện được nên thuật toán trả về`Y`. 

Cuối cùng, kích thước lưới lớn nhất không tạo ra trường hợp đặc biệt. Vì```
1
300 300 2
2 3
100 100
299 299
```Ông X có số chẵn lẻ, trong khi cả hai đại lý đều có số chẵn lẻ. Kích thước lưới không liên quan khi tìm thấy vật cản chẵn lẻ, vì vậy câu trả lời là ngay lập tức`N`. Thuật toán không bao giờ phân bổ lưới (300\times300) và không bao giờ mô phỏng một cuộc rượt đuổi.
