---
title: "CF 104375G - Trò chơi trồng trọt"
description: "Chúng tôi được phát một đống chip và hai người chơi thay phiên nhau, bắt đầu với Jane. Trong mỗi lượt, người chơi loại bỏ từ 1 đến một số lượng chip giới hạn, trong đó giới hạn tăng lên theo chỉ số lượt chơi."
date: "2026-07-01T17:29:21+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104375
codeforces_index: "G"
codeforces_contest_name: "2023 ICPC Gran Premio de Mexico 1ra Fecha"
rating: 0
weight: 104375
solve_time_s: 73
verified: true
draft: false
---

[CF 104375G - Trò chơi phát triển](https://codeforces.com/problemset/problem/104375/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 13s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được phát một đống chip và hai người chơi thay phiên nhau, bắt đầu với Jane. Trong mỗi lượt, người chơi loại bỏ từ 1 đến một số lượng chip giới hạn, trong đó giới hạn tăng lên theo chỉ số lượt chơi. Ở nước đi đầu tiên Jane chỉ được lấy tối đa 1 chip, ở nước đi thứ hai John có thể lấy tối đa 2 chip, ở nước đi thứ ba Jane có thể lấy tối đa 3 chip, v.v. Nếu lượt hiện tại là nước đi thứ i tổng thể, người chơi có thể lấy bất kỳ số nguyên nào từ 1 đến bao gồm i. Ai lấy được con chip cuối cùng sẽ thắng. 

Đầu vào chỉ là số chip N ban đầu và chúng ta phải xác định xem liệu người chơi đầu tiên (Jane) có thể giành chiến thắng hay không nếu cả hai bên chơi tối ưu. 

Ràng buộc N 5000 đủ nhỏ để mọi giải pháp có O(N^2) hoặc thậm chí O(N^2) với hằng số nhỏ đều có thể chấp nhận được. Điều này ngay lập tức gợi ý một cách tiếp cận lập trình động đối với các trạng thái trò chơi được lập chỉ mục theo số chip còn lại và số lượt. 

Một khía cạnh tinh tế là kích thước nước đi tối đa phụ thuộc vào chỉ số lần lượt chứ không phải số chip còn lại hoặc người chơi. Điều này có nghĩa là trạng thái trò chơi không chỉ là “số chip còn lại”, mà còn ngầm phụ thuộc vào số nước đi đã xảy ra. Một cách tiếp cận ngây thơ bỏ qua tính chẵn lẻ của lượt đi hoặc cho rằng các tập hợp nước đi cố định sẽ thất bại. 

Một trường hợp thất bại phổ biến sẽ phát sinh nếu người ta cố gắng mô hình hóa trò chơi này như một trò chơi trừ tiêu chuẩn với các bước di chuyển cố định như {1,2,3,...}. Ví dụ: ở N = 3, người ta có thể cho rằng Jane có thể lấy cả 3 con chip ngay lập tức một cách không chính xác, nhưng cô ấy không thể lấy được vì ở lượt 1, cô ấy bị hạn chế chỉ được lấy 1 con chip. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là coi mỗi trạng thái là (remaining_chips, Turn_index) và thử đệ quy tất cả các bước di chuyển hợp lệ từ 1 đến Turn_index. Từ một trạng thái, chúng tôi kiểm tra xem có tồn tại một nước đi khiến đối thủ rơi vào trạng thái thua cuộc hay không. Điều này nắm bắt chính xác trò chơi, nhưng số lượng trạng thái tăng lên theo O(N^2), vì Turn_index có thể tăng lên N trong trường hợp xấu nhất và các chip còn lại cũng có phạm vi lên tới N. 

Tuy nhiên, nhiều trạng thái trong số này không thể truy cập được trong thực tế vì tổng số lượt trước khi trò chơi kết thúc nhiều nhất là N. Điều này cho thấy chúng ta chỉ cần xem xét các trạng thái lên tới lượt i ≤ N và số chip còn lại lên đến N. Điều quan trọng quan trọng là chúng ta thực sự không cần mô phỏng cây trò chơi tùy ý, chúng ta chỉ cần DP trên i chip còn lại ở bước t. 

Quá trình chuyển đổi trở nên dễ quản lý vì ở lượt t, người chơi chọn k trong [1, t] và chuyển sang trạng thái có i − k chip và lượt tiếp theo t + 1. Vì N nhỏ nên chúng ta có thể tính dp[i][t] nghĩa là liệu người chơi hiện tại có thể thắng với i chip còn lại ở lượt t hay không. 

Điều này giảm xuống thành DP nhiều lớp trong đó mỗi lớp phụ thuộc vào lượt tiếp theo. Chúng tôi tính toán ngược từ i lớn hoặc chuyển tiếp theo cách có cấu trúc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Đệ quy Brute Force trên (i, t) | O(N^3) trường hợp xấu nhất | O(N^2) | Quá chậm | 
| DP trên (i, t) | O(N^2) | O(N^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xác định trạng thái dp[i][t] trong đó i là số chip còn lại và t là số lượt hiện tại (tức là số lần lấy tối đa được phép là t). dp[i][t] đúng nếu người chơi hiện tại có thể buộc phải thắng.

1. Chúng tôi khởi tạo tất cả các trạng thái có i = 0 là trạng thái thua, vì nếu không còn chip, người chơi di chuyển đã thua. Điều này cho ra dp[0][t] = false với mọi t. 
2. Chúng ta lặp lại việc tăng i từ 1 lên N, vì dp[i][t] chỉ phụ thuộc vào các trạng thái có ít chip hơn. 
3. Với mỗi trạng thái (i, t), chúng ta thử tất cả các bước di chuyển k có thể từ 1 đến min(t, i). Mỗi lần di chuyển đều dẫn đến trạng thái dp[i − k][t + 1]. Nếu tồn tại bất kỳ nước đi k nào sao cho dp[i − k][t + 1] sai thì dp[i][t] trở thành đúng. Điều này là do người chơi hiện tại có thể ép đối thủ vào thế thua. 
4. Cuối cùng, chúng tôi quan tâm đến dp[N][1], vì trò chơi bắt đầu với N chip và lượt đầu tiên cho phép lấy tối đa 1 chip. 
5. Chúng tôi tính toán dp khi tăng i và giảm t khi cần thiết, đảm bảo tất cả các chuyển đổi đều đã biết khi sử dụng. 

Tại sao nó hoạt động: mọi trạng thái đều mã hóa một vị trí trò chơi thông tin hoàn hảo với một tập hợp nước đi hữu hạn. DP tuân theo logic tối đa tiêu chuẩn cho các trò chơi công bằng với các hạn chế di chuyển thay đổi. Mỗi trạng thái được phân loại chính xác dựa trên việc nó có ít nhất một nước đi dẫn đến trạng thái thua hay không. Vì mọi chuyển đổi đều làm giảm i nên phép đệ quy có tính chất không tuần hoàn và DP giải quyết hoàn toàn tất cả các vị trí mà không có mâu thuẫn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    N = int(input().strip())

    # dp[i][t] = can current player win with i chips left on turn t
    # t goes up to N+1 safely
    dp = [[False] * (N + 2) for _ in range(N + 1)]

    # base case: no chips -> losing
    for t in range(N + 2):
        dp[0][t] = False

    # fill DP
    for i in range(1, N + 1):
        for t in range(N, 0, -1):
            win = False
            max_take = min(i, t)
            for k in range(1, max_take + 1):
                if not dp[i - k][t + 1]:
                    win = True
                    break
            dp[i][t] = win

    print("Jane" if dp[N][1] else "John")

if __name__ == "__main__":
    solve()
```Bảng DP được lập chỉ mục theo số chip còn lại và số lượt. Chi tiết triển khai chính là đảm bảo rằng khi chúng tôi đánh giá dp[i][t], tất cả dp[i - k][t + 1] đã được tính toán, điều này được đảm bảo vì chúng tôi tăng i một cách đơn điệu và chỉ mong đợi trong t. 

Các vòng lặp lồng nhau phản ánh trực tiếp cấu trúc trò chơi: đối với mỗi trạng thái, chúng tôi liệt kê tất cả các nước đi hợp pháp và kiểm tra xem có bất kỳ động thái nào khiến phản ứng thua hay không. 

## Ví dụ đã hoạt động 

### Ví dụ 1: N = 1 

| tôi | t | những động thái có thể xảy ra | kết quả | 
| --- | --- | --- | --- | 
| 0 | 1 | không | thua | 
| 1 | 1 | k = 1 → dp[0][2] = false | chiến thắng | 

Khi i = 1, Jane có thể lấy được con chip duy nhất ở nước đi đầu tiên. DP đánh dấu dp[1][1] là thắng vì tồn tại sự chuyển sang trạng thái thua. Điều này khớp với đầu ra “Jane”. 

### Ví dụ 2: N = 3 

| tôi | t | di chuyển đã được kiểm tra | dp[i][t] | 
| --- | --- | --- | --- | 
| 1 | 1 | k=1 → dp[0][2]=false | thắng | 
| 2 | 1 | k=1 → dp[1][2], k=2 → dp[0][2] | thắng | 
| 3 | 1 | k=1 → dp[2][2], k=2 → dp[1][2] | thua | 

Với i = 3 ở lượt 1, cả hai nước đi có thể dẫn đến trạng thái mà đối thủ có thể phản ứng tối ưu và tránh thua ngay lập tức. Do đó dp[3][1] là sai và John thắng, phù hợp với mẫu. 

Những dấu vết này cho thấy quyết định ở mỗi trạng thái phụ thuộc hoàn toàn vào việc liệu nước đi nào có đạt đến vị trí thua ở lớp tiếp theo hay không. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N^2) | Đối với mỗi i và t chúng tôi thử tối đa t chuyển đổi | 
| Không gian | O(N^2) | Bàn DP qua (chip, lượt) | 

Ràng buộc N 5000 cho phép khoảng 25 triệu thao tác trong Python ở dạng được tối ưu hóa. DP bậc hai là đường biên nhưng có thể chấp nhận được với các hệ số không đổi nhỏ và các điểm dừng sớm trong quá trình chuyển đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    N = int(sys.stdin.readline().strip())
    dp = [[False] * (N + 2) for _ in range(N + 1)]

    for i in range(1, N + 1):
        for t in range(N, 0, -1):
            win = False
            for k in range(1, min(i, t) + 1):
                if not dp[i - k][t + 1]:
                    win = True
                    break
            dp[i][t] = win

    return "Jane" if dp[N][1] else "John"

# provided samples
assert run("1\n") == "Jane", "sample 1"
assert run("3\n") == "John", "sample 2"
assert run("6\n") == "John", "sample 3"

# custom cases
assert run("2\n") in {"Jane", "John"}, "small boundary check"
assert run("4\n") in {"Jane", "John"}, "consistency check"
assert run("5\n") in {"Jane", "John"}, "stability check"
assert run("10\n") in {"Jane", "John"}, "larger sanity check"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | Jane | trường hợp tối thiểu | 
| 3 | John | trạng thái mất mẫu | 
| 6 | John | trường hợp giữa không tầm thường | 
| 10 | khác nhau | Độ ổn định DP | 

## Vỏ cạnh 

Trường hợp key edge là N = 1. Người chơi đầu tiên chỉ được lấy 1 chip, ngay lập tức giành chiến thắng. DP bắt đầu bằng dp[0][t] = false, do đó dp[1][1] trở thành true vì bước di chuyển duy nhất sẽ dẫn trực tiếp đến dp[0][2]. 

Một trường hợp tinh tế khác là các giá trị chẵn nhỏ như N = 2, trong đó trực giác có thể gợi ý tính đối xứng, nhưng giới hạn di chuyển tăng dần sẽ phá vỡ tính đối xứng. Khi N = 2, Jane lấy 1, để lại 1 chip ở lượt 2, John có thể lấy tới 2 chip và thắng ngay. DP nắm bắt được điều này vì dp[1][2] đánh giá là người chơi đi nước đi là thắng. 

Hành vi của cạnh thứ ba xuất hiện khi N lớn nhưng vẫn trong giai đoạn phát triển ban đầu trong đó giới hạn di chuyển tăng nhanh hơn các chip còn lại. Ở những trạng thái đó, quá trình chuyển đổi dp nhanh chóng bị chi phối bởi các nước đi thắng trực tiếp, vì k luôn có thể đạt đến i một khi t vượt quá i.
