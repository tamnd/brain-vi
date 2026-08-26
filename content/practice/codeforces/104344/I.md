---
title: "CF 104344I - Fila da cantina"
description: "Chúng ta có một hàng trẻ em, trong đó mỗi vị trí đã có sẵn một trẻ em, nhưng mỗi trẻ em có một vị trí mục tiêu mà chúng phải chiếm giữ."
date: "2026-07-01T18:29:53+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104344
codeforces_index: "I"
codeforces_contest_name: "Maratona dos Bixes 2023 - UNICAMP"
rating: 0
weight: 104344
solve_time_s: 61
verified: true
draft: false
---

[CF 104344I - Fila da cantina](https://codeforces.com/problemset/problem/104344/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 1s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một hàng trẻ em, trong đó mỗi vị trí đã có sẵn một trẻ em, nhưng mỗi trẻ em có một vị trí mục tiêu mà chúng phải chiếm giữ. Mảng đầu vào mô tả ánh xạ này: đứa trẻ hiện đang ngồi ở vị trí chỉ mục`i`muốn kết thúc ở vị trí`p[i]`sau khi mọi thứ được sắp xếp chính xác. 

Mục tiêu là biến sự sắp xếp hiện tại thành một sự sắp xếp hoàn hảo, trong đó đứa trẻ phải vào đúng vị trí.`1`ở đó, đứa trẻ lẽ ra phải ở đúng vị trí`2`ở đó, v.v. cho đến vị trí`N`. Hoạt động duy nhất được phép là hoán đổi hai đứa trẻ bất kỳ trong hàng. Chúng tôi muốn số lần hoán đổi tối thiểu cần thiết để đạt được cấu hình chính xác. 

Đây là một bài toán cổ điển “sắp xếp lại một hoán vị bằng cách sử dụng hoán đổi”. Quan sát quan trọng là mảng thực sự là một hoán vị của`1..N`, bởi vì mọi vị trí đúng đều được gán duy nhất. 

Các ràng buộc đủ nhỏ để một nghiệm có hành vi bậc hai trong`N`vẫn sẽ vượt qua trong thực tế. Với`N ≤ 1000`, thậm chí một thuật toán thực hiện theo thứ tự`N^2`hoặc`N^2 log N`hoạt động có thể chấp nhận được. Điều này ngay lập tức loại trừ bất kỳ điều gì theo cấp số nhân hoặc liên quan đến việc tính toán lại sâu lặp đi lặp lại cho mỗi thao tác. 

Một sai lầm ngây thơ nhưng phổ biến là thử mô phỏng các giao dịch hoán đổi một cách tham lam mà không có cấu trúc nhất quán. Ví dụ: liên tục quét tìm vị trí không chính xác tiếp theo và hoán đổi vị trí đó vào vị trí có thể hoạt động nhưng sẽ dễ xảy ra lỗi nếu người ta quên rằng việc hoán đổi có thể tạo ra những sai lệch mới ở nơi khác. Một ý tưởng sai lầm khác là luôn hoán đổi một phần tử sai với phần tử hiện tại ở vị trí đích của nó mà không theo dõi xem phần tử đó thực sự ở đâu, điều này sẽ bị hỏng nếu các vị trí không được lập chỉ mục cẩn thận. 

Các trường hợp cạnh có xu hướng phá vỡ các giải pháp ngây thơ bao gồm các mảng đã được sắp xếp như`[1,2,3,4]`, trong đó câu trả lời bằng 0 và các chu kỳ thuần túy như`[2,3,1]`, trong đó mọi phần tử bị đặt sai vị trí nhưng được cấu trúc theo một chu trình duy nhất. Một giao dịch hoán đổi tham lam bất cẩn có thể tính quá nhiều hoặc quá thấp các giao dịch hoán đổi tùy thuộc vào việc chu kỳ bị phá vỡ như thế nào. 

Cấu trúc sâu hơn ở đây là mảng xác định một hoán vị và các hoán vị phân rã thành các chu trình rời rạc. Các giao dịch hoán đổi tối thiểu cần thiết được xác định hoàn toàn bởi cấu trúc chu kỳ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ liên tục sửa vị trí không chính xác đầu tiên bằng cách tìm phần tử thuộc về vị trí đó và hoán đổi nó vào vị trí. Mỗi lần hoán đổi làm giảm số lượng phần tử bị đặt sai vị trí cục bộ nhưng yêu cầu tìm kiếm nhiều lần. Trong trường hợp xấu nhất, mỗi vị trí yêu cầu quét mảng để xác định đúng phần tử, dẫn đến$O(N)$quét lặp đi lặp lại$O(N)$lần, cho$O(N^2)$thời gian. Điều này có thể chấp nhận được đối với$N ≤ 1000$, nhưng về mặt khái niệm thì phương pháp này lộn xộn và dễ thực hiện không chính xác khi việc hoán đổi ảnh hưởng đến các tìm kiếm trong tương lai. 

Cái nhìn sâu sắc quan trọng là ngừng suy nghĩ về các giao dịch hoán đổi riêng lẻ và thay vào đó hãy xem mảng dưới dạng biểu đồ hoán vị. Mỗi chỉ mục trỏ đến vị trí mà giá trị hiện tại của nó sẽ đi tới. Điều này tạo ra các cạnh có hướng từ`i → p[i]`. Mỗi nút thuộc về đúng một chu kỳ. Một chu kỳ dài`k`yêu cầu chính xác`k - 1`hoán đổi để sửa lỗi, vì mỗi hoán đổi có thể đặt chính xác một phần tử trong khi vẫn giữ nguyên cấu trúc trong chu trình. 

Tính tổng số này qua tất cả các chu kỳ sẽ cho số lần hoán đổi tối thiểu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (sửa nhiều lần) | O(N2) | O(N) | Chấp nhận nhưng vụng về | 
| Phân hủy chu kỳ | O(N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Giải thích mảng như một hoán vị trong đó vị trí`i`cuối cùng phải chứa giá trị`i`. 

Việc sắp xếp lại này cho phép chúng ta suy luận về tính đúng đắn về mặt chỉ số thay vì các giao dịch hoán đổi tùy ý. 
2. Xây dựng một mảng đã truy cập để theo dõi những chỉ mục nào đã được gán cho một chu trình. 

Nếu không có điều này, chúng tôi sẽ liên tục xử lý lại cấu trúc tương tự và các giao dịch hoán đổi vượt mức. 
3. Lặp lại từng chỉ mục từ`1`ĐẾN`N`. 

Nếu chỉ mục đã được truy cập thì nó thuộc về một chu trình được xử lý trước đó, vì vậy chúng tôi bỏ qua nó. 
4. Khi chúng tôi tìm thấy một chỉ mục chưa được truy cập, hãy bắt đầu theo chuỗi`i → p[i] → p[p[i]] → ...`cho đến khi chúng ta quay trở lại nút đã truy cập. 

Việc duyệt này khám phá ra một chu trình hoán vị đầy đủ. 
5. Đếm kích thước`k`của chu kỳ này. 

Mỗi chu kỳ thể hiện một vòng phụ thuộc khép kín gồm các phần tử bị đặt sai vị trí. 
6. Thêm`k - 1`để trả lời. 

Đây là số lần hoán đổi tối thiểu cần thiết để chia một chu kỳ thành các điểm cố định được đặt chính xác. 
7. Tiếp tục cho đến khi tất cả các chỉ số được truy cập và tính tổng tất cả các đóng góp. 

### Tại sao nó hoạt động 

Mọi hoán vị có thể được phân tách duy nhất thành các chu trình rời rạc. Bên trong một chu kỳ có độ dài`k`, không phần tử nào có thể đạt đến đúng vị trí của nó nếu không tương tác với các phần tử khác trong cùng một chu trình. Một lần hoán đổi duy nhất có thể đặt chính xác tối đa một phần tử trong chu trình trong khi vẫn giữ được cấu trúc chu trình nhỏ hơn trong số các phần tử còn lại. Lặp lại điều này một cách tối ưu mang lại chính xác`k - 1`hoán đổi và tính tổng theo các chu kỳ độc lập đảm bảo không có sự can thiệp giữa chúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    p = list(map(int, input().split()))
    
    # convert to 0-based target interpretation
    p = [x - 1 for x in p]
    
    visited = [False] * n
    ans = 0
    
    for i in range(n):
        if visited[i]:
            continue
        
        # explore cycle starting at i
        cur = i
        cycle_size = 0
        
        while not visited[cur]:
            visited[cur] = True
            cycle_size += 1
            cur = p[cur]
        
        if cycle_size > 0:
            ans += cycle_size - 1
    
    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp trước tiên chuyển đổi hoán vị thành lập chỉ mục dựa trên 0 để các vị trí căn chỉnh tự nhiên với các chỉ mục mảng. Mảng đã truy cập đảm bảo mỗi chỉ mục được xử lý chính xác một lần, ngăn chặn chu kỳ đếm kép. Mỗi lần truyền tải chu kỳ tuân theo ánh xạ hoán vị cho đến khi nó lặp lại và kích thước chu trình xác định trực tiếp số lần hoán đổi được thêm vào. 

Phép trừ`cycle_size - 1`là sự chuyển đổi quan trọng từ cơ cấu sang chi phí. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4
2 1 4 3
```| Bước | Bắt đầu | Truyền tải chu kỳ | Kích thước chu kỳ | Đã thêm giao dịch hoán đổi | Tổng cộng | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 0 → 1 → 0 | 2 | 1 | 1 | 
| 2 | 2 | 2 → 3 → 2 | 2 | 1 | 2 | 

Hoán vị chia thành hai chu kỳ độc lập có kích thước 2. Mỗi chu kỳ đóng góp chính xác một lần hoán đổi, phù hợp với trực giác rằng mỗi cặp chỉ cần một lần hoán đổi duy nhất để sửa. 

### Mẫu 2 

đầu vào:```
4
1 2 3 4
```| Bước | Bắt đầu | Truyền tải chu kỳ | Kích thước chu kỳ | Đã thêm giao dịch hoán đổi | Tổng cộng | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 0 | 1 | 0 | 0 | 
| 2 | 1 | 1 | 1 | 0 | 0 | 
| 3 | 2 | 2 | 1 | 0 | 0 | 
| 4 | 3 | 3 | 1 | 0 | 0 | 

Mọi phần tử đều đã ở trong chu kỳ điểm cố định có kích thước 1, do đó không cần hoán đổi. 

Những ví dụ này xác nhận rằng thuật toán phân biệt chính xác giữa các điểm cố định và các chu trình không tầm thường. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Mỗi chỉ mục được truy cập chính xác một lần trong khi hình thành chu kỳ | 
| Không gian | O(N) | Đã truy cập mảng và lưu trữ đầu vào | 

Với`N ≤ 1000`, thuật toán chạy tốt trong giới hạn. Ngay cả trong trường hợp xấu nhất của một chu kỳ kích thước`N`, quá trình truyền tải là tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    
    import builtins
    backup = builtins.input
    builtins.input = lambda: sys.stdin.readline().rstrip("\n")
    
    try:
        solve()
    finally:
        builtins.input = backup
    
    return output.getvalue().strip()

# provided samples
assert run("4\n2 1 4 3\n") == "2", "sample 1"
assert run("4\n1 2 3 4\n") == "0", "sample 2"
assert run("3\n2 3 1\n") == "2", "sample 3"

# custom cases
assert run("1\n1\n") == "0", "single element"
assert run("5\n2 3 4 5 1\n") == "4", "single large cycle"
assert run("6\n1 3 2 5 6 4\n") == "3", "multiple cycles"
assert run("4\n4 3 2 1\n") == "2", "reverse order"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`|`0`| trường hợp nhỏ nhất | 
|`5-cycle shift`|`4`| xử lý toàn bộ chu trình | 
|`6 mixed cycles`|`3`| nhiều chu kỳ rời rạc | 
|`reverse`|`2`| phân rã chu trình đối xứng | 

## Vỏ cạnh 

Đối với đầu vào đã được sắp xếp như`1 2 3 4`, quá trình duyệt ngay lập tức đánh dấu mỗi chỉ mục là một chu kỳ có kích thước 1. Không có giao dịch hoán đổi nào được tích lũy và đầu ra vẫn bằng 0. 

Đối với một chu kỳ đầy đủ như`2 3 4 1`, thuật toán sau`0 → 1 → 2 → 3 → 0`, tạo ra chu trình cỡ 4 và đóng góp`3`trao đổi. Điều này khớp với trình tự tối thiểu trong đó mỗi lần hoán đổi dần dần sửa một phần tử trong khi thu hẹp chu kỳ còn lại. 

Đối với nhiều chu kỳ độc lập như`2 1 4 3 5`, cơ chế được truy cập đảm bảo mỗi chu kỳ được cách ly. Các quá trình thuật toán`(0,1)`Và`(2,3)`riêng biệt, mỗi người đóng góp một trao đổi, trong khi điểm cố định`4`không đóng góp gì
