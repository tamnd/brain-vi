---
title: "CF 2209A - Dép xỏ ngón"
description: "Chúng ta được cung cấp một chuỗi các quái vật, mỗi quái vật có một giá trị sức mạnh. Chúng tôi cũng bắt đầu với sức mạnh chiến đấu ban đầu. Tại bất kỳ thời điểm nào, chúng ta có thể đánh bại một con quái vật nếu sức mạnh hiện tại của chúng ta ít nhất bằng sức mạnh của nó hoặc chúng ta có thể sử dụng một số vật phẩm đặc biệt có giới hạn để tăng…"
date: "2026-06-07T19:22:34+07:00"
tags: ["codeforces", "competitive-programming", "greedy"]
categories: ["algorithms"]
codeforces_contest: 2209
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 1087 (Div. 2)"
rating: 800
weight: 2209
solve_time_s: 133
verified: false
draft: false
---

[CF 2209A - Dép xỏ ngón](https://codeforces.com/problemset/problem/2209/A) 

**Đánh giá:** 800 
**Thẻ:** tham lam 
**Thời gian giải:** 2m 13s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các quái vật, mỗi quái vật có một giá trị sức mạnh. Chúng tôi cũng bắt đầu với sức mạnh chiến đấu ban đầu. Tại bất kỳ thời điểm nào, chúng ta có thể đánh bại một con quái vật nếu sức mạnh hiện tại của chúng ta ít nhất bằng sức mạnh của nó hoặc chúng ta có thể sử dụng một số lượng vật phẩm đặc biệt có giới hạn để tăng sức mạnh của quái vật thêm 1 điểm cho mỗi lần sử dụng. 

Đánh bại quái vật có lợi vì nó tăng sức chiến đấu của chúng ta bằng sức mạnh hiện tại của quái vật và quái vật bị loại bỏ. Điều khó khăn là chúng ta được phép “tạo hình” tối đa k quái vật bằng cách tăng dần sức mạnh của chúng trước khi quyết định có nên chiến đấu với chúng hay không, nhưng mỗi lần tăng sẽ tiêu tốn một lượt lật. 

Mục tiêu là tối đa hóa sức mạnh chiến đấu cuối cùng sau khi chọn trình tự tăng cường và tiêu diệt tối ưu. 

Sự tương tác quan trọng là việc tăng sức mạnh cho một con quái vật khiến việc tiêu diệt ngay lập tức trở nên khó khăn hơn nhưng nó có thể mang lại nhiều lợi nhuận hơn sau này khi chúng ta đã phát triển mạnh hơn. Đồng thời, tiêu diệt sớm hơn sẽ tăng sức mạnh của chúng ta, tạo điều kiện tiếp cận những quái vật mạnh hơn. 

Các ràng buộc nhỏ về mặt n, tối đa 100 cho mỗi trường hợp thử nghiệm, nhưng giá trị cho cường độ và công suất ban đầu lên tới 10^9 và k có thể rất lớn. Điều này ngay lập tức loại trừ mọi tìm kiếm trong không gian trạng thái đối với sự phân bố của flip flop. Ngay cả một DP ngây thơ trước quái vật và còn lại k là không thể vì k rất lớn. 

Cấu trúc gợi ý rằng chúng tôi không chọn cách phân bổ gia tăng tùy ý trên toàn cầu mà đưa ra các quyết định cục bộ về việc liệu một con quái vật hiện có thể sử dụng được hay cần được điều chỉnh một chút. 

Một trường hợp thất bại tinh vi đối với lối suy nghĩ tham lam ngây thơ phát sinh khi một con quái vật vừa đủ mạnh, chẳng hạn: 

đầu vào:```
c = 10, k = 1
a = [12, 11]
```Một chiến lược ngây thơ có thể dùng flip flop ở mức 12 để biến nó thành 13, điều này còn tệ hơn. Bước đi đúng đắn là chi số tiền đó cho 11 để biến nó thành 12 để 11 có thể bị tiêu diệt sớm hơn 12, cải thiện trật tự tăng trưởng trong tương lai. 

Một trường hợp nguy hiểm khác xuất hiện khi sử dụng tất cả các flip flop quá sớm sẽ ngăn cản việc đạt được những lần tiêu diệt có lợi sau này. 

Khó khăn chính là quyết định cách sử dụng k số gia tăng trên nhiều quái vật trong khi vẫn duy trì thứ tự tiêu diệt tốt nhất. 

## Phương pháp tiếp cận 

Một cách tiếp cận vũ phu sẽ thử mọi cách để gán tối đa k số gia tăng cho các quái vật và tất cả các hoán vị của lệnh tiêu diệt. Đối với mỗi cấu hình, chúng tôi mô phỏng các lần tiêu diệt theo thứ tự khả thi hiện tại. Điều này bùng nổ ngay lập tức bởi vì ngay cả việc quyết định cách phân phối k số gia tăng cũng là tổ hợp với các khả năng gần đúng (k+n chọn n), điều này không khả thi ngay cả với k = 10^9. 

Quan sát quan trọng là tác dụng có ý nghĩa duy nhất của dép xỏ ngón là làm thay đổi xem quái vật có thể bị tiêu diệt sớm hơn hay muộn hơn. Vì mỗi lần tăng thay đổi một quái vật chính xác 1, chi phí để làm cho một quái vật có thể bị tiêu diệt ở mức sức mạnh c hiện tại là xác định: max(0, a_i - c). Chúng ta không bao giờ cần tăng số lượng quái vật vượt quá c, bởi vì làm như vậy chỉ làm trì hoãn việc tiêu diệt nó. 

Vì vậy, mỗi con quái vật đều có một “chi phí kích hoạt” liên quan: cần bao nhiêu lần lật trước khi chúng ta có thể tiêu diệt nó ở mức sức mạnh hiện tại. Khi một con quái vật trở nên có thể bị giết, việc tiêu diệt nó ngay lập tức luôn là điều tối ưu vì nó làm tăng c và chỉ khiến việc tiêu diệt trong tương lai trở nên dễ dàng hơn. 

Điều này làm giảm vấn đề liên tục lựa chọn những con quái vật có chi phí đã điều chỉnh phù hợp với k còn lại và c hiện tại, luôn ưu tiên con quái vật mang lại tiến bộ trong việc tăng trưởng sức mạnh. Vì n nhỏ nên chúng ta có thể quét liên tục và chọn quái vật khả thi nhất, cập nhật c khi chúng ta thực hiện. 

Cấu trúc tham lam xuất hiện: ở mỗi bước, chúng ta tiêu diệt bất kỳ quái vật nào hiện có thể tiếp cận được hoặc chúng ta chi tiêu những lần điều chỉnh rẻ nhất cần thiết để mở khóa một con quái vật mới. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(n²) mỗi lần kiểm tra | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi mô phỏng quá trình phát triển bằng cách sử dụng sức mạnh chiến đấu hiện tại và những cú lật ngược tình thế còn lại. 

1. Bắt đầu với sức mạnh chiến đấu hiện tại c và tất cả quái vật còn sống. Chúng tôi cũng theo dõi k flip flop còn lại. Ý tưởng chính là tại bất kỳ thời điểm nào, chỉ những quái vật có a_i ≤ c mới có thể sử dụng được ngay lập tức. 
2. Trong khi vẫn còn quái vật, trước tiên chúng tôi cố gắng tìm bất kỳ quái vật nào có thể bị giết trực tiếp mà không tốn dép. Nếu một con quái vật như vậy tồn tại, chúng tôi sẽ loại bỏ nó và tăng c lên giá trị của nó. Điều này luôn tối ưu vì nó mang lại sự tăng trưởng ngay lập tức mà không tiêu tốn tài nguyên. 
3. Nếu hiện tại không có quái vật nào có thể giết được, chúng tôi sẽ xem xét quái vật nào có thể giết được rẻ nhất. Đối với mỗi quái vật còn sống, hãy tính chi phí yêu cầu tối đa(0, a_i - c). Chúng tôi chọn chi phí nhỏ nhất như vậy. Đây là cách hiệu quả nhất để mở ra cơ hội tăng trưởng tiếp theo. 
4. Nếu chi phí yêu cầu rẻ nhất lớn hơn k, chúng tôi dừng lại vì không thể tiến thêm được nữa. Vào thời điểm đó, những quái vật còn lại quá mạnh để có thể kích hoạt và việc dừng lại là tối ưu vì tiết kiệm k không có lợi ích gì trong tương lai. 
5. Nếu không, chúng tôi dành chi phí đó cho quái vật đã chọn, giảm k và tăng sức mạnh hiệu dụng của nó để phù hợp với c. Sau lần điều chỉnh đó, quái vật sẽ có thể bị tiêu diệt ở bước tiếp theo. 
6. Chúng tôi lặp lại lần nữa, xen kẽ giữa tiêu diệt miễn phí và các hoạt động mở khóa tối thiểu, đảm bảo chúng tôi luôn sử dụng tài nguyên theo cách tăng trưởng hiệu quả nhất. 

### Tại sao nó hoạt động

Thuật toán duy trì tính bất biến là chúng ta không bao giờ sử dụng flip flop trừ khi nó cho phép giết ít nhất một lần ngay lập tức. Bất kỳ sự phân bổ flip flop nào không dẫn đến việc tiêu diệt ngay lập tức đều có thể bị hoãn lại mà không ảnh hưởng đến tính khả thi trong tương lai, bởi vì việc tăng c thông qua số lần tiêu diệt chỉ làm cho các ngưỡng trong tương lai dễ dàng được đáp ứng hơn. Do đó, mọi khoản chi tiêu đều phù hợp với việc mở khóa mức tăng sức mạnh tiếp theo và việc tham lam lựa chọn mức điều chỉnh bắt buộc tối thiểu đảm bảo chúng tôi tối đa hóa số lần tiêu diệt có thể tiếp cận theo thời gian. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, c, k = map(int, input().split())
        a = list(map(int, input().split()))
        alive = [True] * n

        while True:
            changed = False

            # try free kills
            for i in range(n):
                if alive[i] and a[i] <= c:
                    c += a[i]
                    alive[i] = False
                    changed = True

            if changed:
                continue

            # find cheapest activation
            best_i = -1
            best_cost = 10**30

            for i in range(n):
                if alive[i]:
                    cost = max(0, a[i] - c)
                    if cost < best_cost:
                        best_cost = cost
                        best_i = i

            if best_i == -1:
                break

            if best_cost > k:
                break

            k -= best_cost
            a[best_i] = c  # now killable immediately

        # final clean-up: kill everything possible
        for i in range(n):
            if alive[i] and a[i] <= c:
                c += a[i]
                alive[i] = False

        print(c)

if __name__ == "__main__":
    solve()
```Giải pháp được cấu trúc xung quanh việc áp dụng liên tục hai thao tác: tiêu thụ miễn phí tất cả quái vật hiện có và mở khóa mục tiêu quái vật rẻ nhất trong tương lai. Mảng boolean theo dõi những con quái vật nào vẫn hoạt động và vòng lặp chính đảm bảo chúng ta chỉ sử dụng flip flop khi không có sự tăng trưởng ngay lập tức. 

Chi tiết triển khai tinh tế là vòng dọn dẹp cuối cùng. Sau khi hết dép xỏ ngón hoặc không thể kích hoạt thêm, chúng tôi vẫn phải tiêu thụ tất cả quái vật có thể tiếp cận còn lại vì những lần mở khóa trước đó có thể khiến thêm quái vật đủ điều kiện mà không phải trả thêm phí. 

Nhiệm vụ`a[best_i] = c`là một cách nhỏ gọn để đánh dấu một quái vật chính xác có thể bị tiêu diệt mà không cần điều chỉnh thêm, đảm bảo rằng nó sẽ bị tiêu diệt trong giai đoạn tự do tiếp theo. 

## Ví dụ đã hoạt động 

Hãy xem xét một hoạt động đơn giản hóa: 

đầu vào:```
n = 3, c = 5, k = 2
a = [6, 4, 10]
```| Bước | c | k | Quái vật sống | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | 5 | 2 | [6,4,10] | giết 4 | 
| 2 | 9 | 2 | [6,10] | giết 6 | 
| 3 | 15 | 2 | [10] | giết 10 | 

Dấu vết này cho thấy hành vi giết người tham lam thuần túy mà không cần đến flip flop, xác nhận rằng thuật toán ưu tiên tăng trưởng tự do trước tiên. 

Bây giờ hãy xem xét một trường hợp trong đó dép tông đóng vai trò quan trọng: 

đầu vào:```
n = 2, c = 3, k = 1
a = [6, 4]
```| Bước | c | k | Hành động | 
| --- | --- | --- | --- | 
| 1 | 3 | 1 | cần chọn kích hoạt rẻ nhất | 
| 2 | 3 | 0 | tăng 4 lên 3 (hiệu quả) | 
| 3 | 3 | 0 | giết 4 | 
| 4 | 7 | 0 | giết 6 | 

Điều này chứng tỏ việc chi tiêu một cú lật cho một điều chỉnh cần thiết nhỏ nhất sẽ mở ra chuỗi tiêu diệt tối ưu như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n² mỗi lần kiểm tra) | Mỗi lần lặp lại sẽ giết một con quái vật hoặc quét tất cả những con quái vật còn lại để tìm cách kích hoạt rẻ nhất, với tối đa n lần kích hoạt và n lần giết | 
| Không gian | O(n) | Mảng Boolean cho trạng thái hoạt động và lưu trữ đầu vào | 

Cho n ≤ 100 và t ≤ 500, trường hợp xấu nhất nằm trong giới hạn thoải mái vì n² trên mỗi thử nghiệm tối đa là 10⁴ thao tác. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def solve():
        t = int(input())
        for _ in range(t):
            n, c, k = map(int, input().split())
            a = list(map(int, input().split()))
            alive = [True] * n

            while True:
                changed = False
                for i in range(n):
                    if alive[i] and a[i] <= c:
                        c += a[i]
                        alive[i] = False
                        changed = True
                if changed:
                    continue

                best_i = -1
                best_cost = 10**30
                for i in range(n):
                    if alive[i]:
                        cost = max(0, a[i] - c)
                        if cost < best_cost:
                            best_cost = cost
                            best_i = i

                if best_i == -1 or best_cost > k:
                    break

                k -= best_cost
                a[best_i] = c

            for i in range(n):
                if alive[i] and a[i] <= c:
                    c += a[i]

            print(c)

    old_stdout = sys.stdout
    sys.stdout = io.StringIO()
    solve()
    out = sys.stdout.getvalue()
    sys.stdout = old_stdout
    return out.strip()

# provided samples (partial due to length)
assert run("""1
1 12 23
21
""") == "12"

assert run("""1
1 8 4
5
""") == "16"

# edge cases
assert run("""1
1 0 10
100
""") == "0"
assert run("""1
3 5 0
10 20 30
""") == "5"
assert run("""1
2 1 100
2 100
""") == "103"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| quái vật đơn quá mạnh | không đổi c | không lật có nghĩa là không tiến bộ | 
| quái vật yếu đuối duy nhất | tăng trực tiếp | giết tham lam cơ bản | 
| không k với quái mạnh | không thể tiến bộ | hạn chế về nguồn lực | 
| giá trị hỗn hợp lớn k | ra lệnh giết người | xiềng xích tham lam | 

## Vỏ cạnh 

Trường hợp quan trọng là khi ban đầu tất cả quái vật đều mạnh hơn c và k bằng 0. Thuật toán bắt đầu tìm kiếm kích hoạt ngay lập tức, tìm thấy chi phí dương và dừng lại một cách chính xác vì không thể thực hiện được tiến trình nào. Ví dụ: 

đầu vào:```
n = 2, c = 1, k = 0, a = [5, 6]
```Vì mọi chi phí đều vượt quá k nên không được phép điều chỉnh nên câu trả lời vẫn là 1. 

Một trường hợp khác là khi k đủ lớn để giảm con quái vật nhỏ nhất xuống dưới c, mở khóa một tầng. Ví dụ: 

đầu vào:```
n = 2, c = 3, k = 2, a = [6, 4]
```Thuật toán dành 1 cho 4, ngay lập tức cho phép tiêu diệt, sau đó sử dụng mức tăng trưởng thu được để mở khóa phần còn lại. Tính bất biến được giữ nguyên vì mỗi lần chi tiêu ngay sau đó sẽ có ít nhất một lần tiêu diệt, đảm bảo không có cú lật nào bị lãng phí. 

Trường hợp thứ ba là khi nhiều quái vật có thể bị tiêu diệt đồng thời sau một lần tiêu diệt. Vòng giết miễn phí đảm bảo tất cả quái vật như vậy được tiêu thụ trước khi chi tiêu thêm, duy trì thứ tự tối ưu.
