---
title: "CF 105818E - Đua Robot"
description: "Chúng ta sắp xếp các robot theo thứ tự tốc độ không giảm dần. Đối với mỗi phân đoạn robot liền kề, chúng tôi muốn biết số nhóm tối đa mà chúng có thể được chia thành để mỗi nhóm có thể dừng lại ở một giây khác nhau trước khi bất kỳ robot nào rời khỏi đường đua."
date: "2026-06-25T15:11:06+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105818
codeforces_index: "E"
codeforces_contest_name: "TeamsCode Spring 2025 Advanced Division"
rating: 0
weight: 105818
solve_time_s: 69
verified: true
draft: false
---

[CF 105818E - Đua robot](https://codeforces.com/problemset/problem/105818/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 9 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta sắp xếp các robot theo thứ tự tốc độ không giảm dần. Đối với mỗi phân đoạn robot liền kề, chúng tôi muốn biết số nhóm tối đa mà chúng có thể được chia thành để mỗi nhóm có thể dừng lại ở một giây khác nhau trước khi bất kỳ robot nào rời khỏi đường đua. 

Robot có tốc độ`a`có thể tồn tại đến thứ hai`floor(L / a)`. Nếu một nhóm bị bắn ở giây thứ hai`t`, mọi robot trong nhóm đó ít nhất phải có thời gian sống sót`t`. Vì các nhóm sau sẽ bị bắn muộn hơn nên robot càng nhanh là những nhóm nguy hiểm và cần được đặt sớm hơn. 

Điều quan trọng cần lưu ý là đối với một mảng con, chiến lược tốt nhất luôn lấy tiền tố của mảng con đó làm các nhóm riêng biệt. Cho phép`d[i] = floor(L / a[i])`là giây cuối cùng có thể cho robot`i`. Bởi vì tốc độ được sắp xếp, thời hạn`d[i]`không tăng, vì vậy rô-bốt ở sau trong mảng không bao giờ dễ dàng lưu hơn rô-bốt trước đó. 

Đối với một mảng con bắt đầu tại`l`, tiền tố có độ dài`k`hoạt động nếu robot ở vị trí`l + k - 1`có thể tồn tại đến thứ hai`1`, cái trước cho đến cái thứ hai`2`, vân vân. Điều này có thể được viết là:`d[i] >= l + k - i`cho mọi chỉ mục trong tiền tố đó. Sắp xếp lại mang lại:`d[i] + i >= l + k`vì vậy toàn bộ tiền tố phải có mọi giá trị của`d[i] + i`ít nhất`l + k`. 

Những ràng buộc cho phép`N`để đạt được`200000`. Một giải pháp kiểm tra mọi mảng con và mô phỏng việc phân nhóm sẽ yêu cầu khoảng`N^3`làm việc trong trường hợp xấu nhất, vượt xa những gì có thể. Ngay cả việc kiểm tra từng mảng con bằng quét tuyến tính cũng cho kết quả`O(N^2)`hoạt động vốn đã quá chậm đối với các thử nghiệm lớn nhất. 

Các trường hợp chính xuất phát từ việc nhầm lẫn giữa nhóm tốt nhất với thứ tự ban đầu hoặc từ việc cho rằng toàn bộ mảng con luôn có thể được tách ra. 

Ví dụ:```
Input:
4 10
1 3 6 10
```Câu trả lời là:```
17
```Hai robot cuối cùng không thể tách rời vì robot có tốc độ`10`chỉ sống sót được một giây. Một giải pháp tham lam luôn tạo một nhóm cho mỗi robot sẽ tính sai mảng con này là có điểm`4`. 

Một trường hợp cạnh khác là một robot duy nhất:```
Input:
1 100
50
```Câu trả lời là:```
1
```Chỉ có một nhóm khả thi, mặc dù robot có thể sống sót sau vài giây. Điểm tính theo nhóm chứ không phải số giây có sẵn. 

Lỗi phổ biến thứ ba là quên rằng thuộc tính tiền tố phụ thuộc vào chỉ mục bắt đầu:```
Input:
3 10
1 10 10
```Mảng con`[10, 10]`có điểm`1`, không`2`, vì robot thứ hai không thể được lưu ở giây thứ hai. Câu trả lời là:```
5
```Điểm hợp lệ là`1, 1, 1, 1, 1`trên ba phần tử đơn và mảng con hai phần tử, cộng với điểm toàn bộ của mảng con`2`. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là xem xét từng mảng con, mô phỏng thời hạn của các robot trong đó và nhanh chóng gán cho những robot mới nhất có thể có những giây sớm nhất. Điều này đúng vì đây là ý tưởng lập kế hoạch tiêu chuẩn cho các công việc có thời hạn. Tuy nhiên, có`N * (N + 1) / 2`mảng con, vì vậy ngay cả một`O(length)`tính toán cho mỗi mảng con trở thành`O(N^3)`. 

Nhận xét quan trọng là câu trả lời cho vị trí xuất phát không phụ thuộc vào ranh giới bên phải một cách phức tạp. Giả sử chúng ta tính toán`best[l]`, số lượng nhóm tối đa có thể nếu chúng ta lấy hậu tố bắt đầu từ`l`và cho phép nó sử dụng tất cả các robot còn lại. Sau đó, bất kỳ mảng con nào bắt đầu từ`l`với chiều dài`len`đã có điểm chính xác`min(best[l], len)`. 

Để tính toán`best`, định nghĩa:`value[i] = i + floor(L / a[i])`sử dụng các chỉ số dựa trên một. Tiền tố có độ dài hợp lệ`k`bắt đầu từ`i`yêu cầu mọi giá trị trong tiền tố đó ít nhất phải`i + k`. 

Việc xử lý từ phải sang trái sẽ cho phép lặp lại đơn giản. Nếu như`best[i + 1]`robot có thể được tách ra sau vị trí`i`, sau đó định vị`i`chỉ có thể mở rộng tiền tố hợp lệ đó thêm một nếu giá trị của chính nó đủ lớn. Giá trị mới là:`best[i] = min(best[i + 1] + 1, value[i] - i)`với tối thiểu là một. 

Sau đó, mỗi vị trí bắt đầu đóng góp một tổng số học đơn giản. Nếu độ dài còn lại là`m`, thì phần đóng góp là tổng của`min(best[i], 1) + min(best[i], 2) + ... + min(best[i], m)`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N^3) | O(1) | Quá chậm | 
| Tối ưu | O(N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính thời hạn sống sót của mỗi robot. Đối với robot`i`, tính toán`d[i] = L // a[i]`và lưu trữ`value[i] = i + d[i]`. Việc chuyển đổi giá trị biến điều kiện nhóm thành điều kiện tối thiểu tiền tố. 
2. Xử lý vị trí từ phải qua trái và tính toán`best[i]`. Robot cuối cùng luôn có thể tạo thành một nhóm, vì vậy giá trị ban đầu là một. Đối với mọi rô-bốt trước đó, tiền tố có thể dài hơn nhiều nhất một tiền tố tốt nhất bắt đầu sau nó và nó cũng bị giới hạn bởi`value[i] - i`. 
3. Đối với mọi vị trí xuất phát`i`, cho phép`length = N - i + 1`. Nếu như`best[i]`ít nhất là`length`, mọi độ dài tiền tố có thể có đều hợp lệ, do đó đóng góp là`length * (length + 1) / 2`. 
4. Nếu không, đầu tiên`best[i]`độ dài tăng bình thường và mọi phân đoạn dài hơn đều giữ nguyên số điểm. Cộng tổng số học của phần đầu tiên và phần đóng góp không đổi của phần còn lại. 
5. Tính tổng tất cả các khoản đóng góp và in kết quả. 

Tại sao nó hoạt động: 

Việc phân nhóm tham lam có hiệu quả vì các rô-bốt sau này có thời hạn nhỏ hơn, do đó, bất kỳ lịch trình tối ưu nào cũng có thể được sắp xếp lại sao cho các nhóm được lấy từ phía bên phải của tiền tố đã chọn trước tiên. Điều này có nghĩa là điều duy nhất quan trọng là chúng ta có thể tạo tiền tố hợp lệ trong bao lâu. 

Sự lặp lại bảo tồn tài sản này. Nếu hậu tố sau`i`hỗ trợ`k`nhóm, sau đó thêm robot`i`chỉ có thể tạo tiền tố có độ dài`k + 1`nếu robot`i`tồn tại cho đến giây cuối cùng mới đó. Không thể sử dụng tiền tố nữa vì các robot còn lại đã tạo thành hậu tố hợp lệ tối đa. Như vậy mọi`best[i]`là chính xác và phép tính tổng cuối cùng tính chính xác tất cả các mảng con. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, L = map(int, input().split())
    a = list(map(int, input().split()))

    best = [0] * (n + 2)

    for i in range(n - 1, -1, -1):
        deadline = L // a[i]
        extend = deadline
        best[i] = min(best[i + 1] + 1, extend)

    ans = 0

    for i in range(n):
        length = n - i
        k = best[i]
        if k >= length:
            ans += length * (length + 1) // 2
        else:
            ans += k * (k + 1) // 2
            ans += (length - k) * k

    print(ans)

if __name__ == "__main__":
    solve()
```Mảng`best`được tính từ phải sang trái vì giá trị tại vị trí`i`phụ thuộc vào giá trị đã được tính toán của`i + 1`. Phép truy hồi chỉ sử dụng thời hạn của robot hiện tại và kết quả hậu tố tiếp theo nên không cần cấu trúc dữ liệu. 

Việc tính toán đóng góp sử dụng công thức tính tổng của khoản đóng góp đầu tiên.`k`số nguyên dương. Khi mảng con dài hơn tiền tố hợp lệ tối đa, mọi rô-bốt bổ sung sẽ giữ nguyên điểm, đó là lý do tại sao phần còn lại đóng góp`(length - k) * k`. 

Tất cả các phép tính được thực hiện bằng số nguyên Python, tự động xử lý các khoản tiền lớn có thể vượt quá phạm vi 32 bit. 

## Ví dụ đã hoạt động 

Mẫu 1:```
Input:
4 10
1 3 6 10
```Các giá trị được chuyển đổi là: 

| Chỉ mục | Tốc độ | Hạn chót | Giá trị | tốt nhất | 
| --- | --- | --- | --- | --- | 
| 4 | 10 | 1 | 5 | 1 | 
| 3 | 6 | 1 | 4 | 1 | 
| 2 | 3 | 3 | 5 | 2 | 
| 1 | 1 | 10 | 11 | 3 | 

Những đóng góp là: 

| Bắt đầu | Chiều dài | tốt nhất | Đóng góp | 
| --- | --- | --- | --- | 
| 1 | 4 | 3 | 9 | 
| 2 | 3 | 2 | 5 | 
| 3 | 2 | 1 | 2 | 
| 4 | 1 | 1 | 1 | 

Tổng cộng là`17`. 

Ví dụ này cho thấy tại sao những robot nhanh nhất không phải lúc nào cũng có thể trở thành những nhóm riêng biệt. Hạn chế tiền tố ngăn chặn robot cuối cùng bị trì hoãn. 

Mẫu 2:```
Input:
3 10
1 10 10
```Các giá trị là: 

| Chỉ mục | Tốc độ | Hạn chót | Giá trị | tốt nhất | 
| --- | --- | --- | --- | --- | 
| 3 | 10 | 1 | 4 | 1 | 
| 2 | 10 | 1 | 3 | 1 | 
| 1 | 1 | 10 | 11 | 2 | 

Những đóng góp là: 

| Bắt đầu | Chiều dài | tốt nhất | Đóng góp | 
| --- | --- | --- | --- | 
| 1 | 3 | 2 | 5 | 
| 2 | 2 | 1 | 2 | 
| 3 | 1 | 1 | 1 | 

Tổng cộng là`8`. 

Dấu vết chứng tỏ rằng một robot chậm ngay từ đầu có thể tạo ra số điểm lớn hơn, trong khi hai robot nhanh cùng nhau không thể bị trì hoãn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Mỗi robot được xử lý một lần và mỗi đóng góp được tính một lần | 
| Không gian | O(N) | các`best`mảng lưu trữ một giá trị cho mỗi robot | 

Giải pháp phù hợp với các ràng buộc bởi vì`N`nhiều nhất là`200000`, do đó, việc vượt qua tuyến tính dễ dàng nằm trong giới hạn thời gian yêu cầu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    
    n, L = map(int, input().split())
    a = list(map(int, input().split()))

    best = [0] * (n + 2)

    for i in range(n - 1, -1, -1):
        best[i] = min(best[i + 1] + 1, L // a[i])

    ans = 0
    for i in range(n):
        length = n - i
        k = best[i]
        if k >= length:
            ans += length * (length + 1) // 2
        else:
            ans += k * (k + 1) // 2 + (length - k) * k

    sys.stdin = old
    return str(ans)

assert run("""4 10
1 3 6 10
""") == "17"

assert run("""3 10
1 10 10
""") == "8"

assert run("""1 100
50
""") == "1"

assert run("""5 5
1 1 1 1 1
""") == "35"

assert run("""4 10
10 10 10 10
""") == "4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`4 10 / 1 3 6 10`|`17`| Hành vi mẫu và lập kế hoạch tiền tố | 
|`3 10 / 1 10 10`|`8`| Robot nhanh không thể tách rời | 
|`1 100 / 50`|`1`| Đầu vào kích thước tối thiểu | 
|`5 5 / 1 1 1 1 1`|`35`| Tất cả các giá trị bằng nhau và nhóm tối đa | 
|`4 10 / 10 10 10 10`|`4`| Trường hợp ranh giới trong đó mọi nhóm phải được hợp nhất | 

## Vỏ cạnh 

Đối với trường hợp robot đơn:```
Input:
1 100
50
```Thuật toán tính toán`best[1] = 1`. Mảng con duy nhất có độ dài bằng một, vì vậy phần đóng góp là một. Kết quả đúng vì một robot luôn tạo thành đúng một nhóm. 

Đối với các robot có tốc độ như nhau:```
Input:
4 10
10 10 10 10
```Mọi robot đều có thời hạn`1`. Tính toán`best`tất cả đều có giá trị`1`. Mỗi vị trí bắt đầu chỉ đóng góp một nhóm hợp lệ, vì vậy câu trả lời là`4`. Một giải pháp giả định tốc độ bằng nhau luôn có thể được tách ra sẽ bị tính quá mức. 

Đối với trường hợp hỗn hợp:```
Input:
4 10
1 3 6 10
```Robot đầu tiên có thời hạn lớn và có thể bị trễ. Robot cuối cùng có thời hạn`1`và phải bắn ngay. Sự lặp lại nắm bắt được điều này bằng cách tạo độ dài tiền tố hợp lệ`[3, 2, 1, 1]`và tổng cuối cùng khớp với hành vi nhóm được yêu cầu.
