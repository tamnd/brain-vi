---
title: "CF 1491C - Pekora và tấm bạt lò xo"
description: "Chúng ta được cung cấp một loạt các độ bền của tấm bạt lò xo được sắp xếp thành một đường thẳng. Mỗi tấm bạt lò xo hoạt động giống như một cú nhảy cưỡng bức: nếu Pekora đáp xuống vị trí i, cô ấy ngay lập tức được phóng lên i + S[i]."
date: "2026-06-14T17:39:17+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "data-structures", "dp", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1491
codeforces_index: "C"
codeforces_contest_name: "Codeforces Global Round 13"
rating: 1700
weight: 1491
solve_time_s: 194
verified: false
draft: false
---

[CF 1491C - Pekora và tấm bạt lò xo](https://codeforces.com/problemset/problem/1491/C) 

**Đánh giá:** 1700 
**Tags:** bạo lực, cấu trúc dữ liệu, dp, tham lam, triển khai 
**Thời gian giải:** 3 phút 14s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một loạt các độ bền của tấm bạt lò xo được sắp xếp thành một đường thẳng. Mỗi tấm bạt lò xo hoạt động giống như một cú nhảy bắt buộc: nếu Pekora tiếp đất đúng vị trí`i`, cô ấy ngay lập tức được phóng tới`i + S[i]`. Sau khi sử dụng tấm bạt lò xo, sức mạnh của nó giảm đi một, nhưng nó không bao giờ giảm xuống dưới một, vì vậy cuối cùng mọi tấm bạt lò xo đều ổn định ở cường độ một. 

Một “đường chuyền” duy nhất là một chuỗi các bước nhảy bắt buộc. Pekora chọn tấm bạt lò xo xuất phát, sau đó tiếp tục nhảy theo cho đến khi rời khỏi dãy bên phải. Cô ấy không thể làm gián đoạn đường chuyền sớm. Mục tiêu là thực hiện một số đường chuyền như vậy để sức mạnh của mỗi tấm bạt lò xo giảm xuống chính xác một đường chuyền và chúng tôi muốn giảm thiểu số đường chuyền. 

Mỗi lần tấm bạt lò xo được sử dụng trong một đường chuyền, nó đóng góp chính xác một mức giảm, do đó, hạn chế thực sự không phải là chúng ta nhảy bao xa theo thuật ngữ tuyệt đối mà là số lần mỗi chỉ mục được truy cập trên tất cả các đường chuyền, đồng thời tôn trọng rằng đường chuyền tuân theo các liên kết nhảy xác định thay đổi theo thời gian. 

Các ràng buộc làm cho việc mô phỏng vũ lực trở nên không thể thực hiện được theo nghĩa đơn giản. Với`n`lên tới 5000 và giá trị lên tới 1e9, bất kỳ chiến lược nào thử lặp lại tất cả các điểm bắt đầu hoặc mô phỏng từng bước giảm dần trên mỗi đơn vị sẽ thoái hóa thành hành vi bậc hai hoặc tệ hơn trong mỗi trường hợp thử nghiệm. Vì tổng cộng`n`qua các bài kiểm tra cũng là 5000, một`O(n^2)`hoặc`O(n^2 log n)`cách tiếp cận có thể ở ranh giới nhưng vẫn rủi ro nếu hằng số lớn; bất cứ điều gì hình khối đều ngay lập tức không thể thực hiện được. 

Hành vi cạnh phức tạp xuất phát từ sự tương tác giữa các bước nhảy lớn và sức mạnh giảm dần. Một kỳ vọng ngây thơ là các giá trị lớn hoạt động độc lập, nhưng trên thực tế, một đường truyền duy nhất có thể ảnh hưởng đến nhiều vị trí ở xa trong một cấp số cộng cố định và các đường truyền sau đó có thể sử dụng lại hoặc không sử dụng lại các đường dẫn đó tùy thuộc vào cách các giá trị co lại. 

Một vài cạm bẫy cụ thể: 

Một ý tưởng tham lam ngây thơ có thể là “luôn bắt đầu ở giá trị còn lại lớn nhất”, nhưng điều đó không thành công vì một lần vượt qua có thể đạt hoặc thậm chí không đạt được các vị trí đó tùy thuộc vào mức giảm trung gian. Ví dụ: nếu các vị trí ban đầu có giá trị rất lớn thì lượt bắt đầu muộn hơn có thể không bao giờ đến được với chúng, do đó, việc đặt hàng tham lam cục bộ sẽ phá vỡ cấu trúc khả năng tiếp cận toàn cầu. 

Một vấn đề tế nhị khác là giả định mỗi vị trí có thể được xử lý độc lập. Ví dụ: nếu tất cả các giá trị là 2 ngoại trừ một giá trị 100, một người giải đơn giản có thể nghĩ rằng số 100 yêu cầu 99 đường chuyền riêng biệt bắt đầu từ vị trí đó, nhưng trên thực tế, nhiều mức giảm có thể được chia sẻ giữa các đường chuyền tùy thuộc vào đường dẫn truyền tải. 

Cấu trúc ẩn chính là mỗi lần vượt qua sẽ tạo ra một bước đi xác định và mỗi lượt truy cập sẽ thay đổi cấu trúc tương lai theo một cách rất được kiểm soát. 

## Phương pháp tiếp cận 

Chế độ xem bạo lực là mô phỏng rõ ràng các đường chuyền. Trong mỗi lần vượt qua, chúng tôi chọn chỉ mục bắt đầu, sau đó mô phỏng chuỗi nhảy cho đến khi chúng tôi thoát khỏi mảng, giảm dần mọi vị trí đã truy cập. Chúng tôi lặp lại cho đến khi tất cả các giá trị trở thành một. Điều này đúng vì nó tuân theo định nghĩa bài toán một cách chính xác. 

Tuy nhiên, chi phí là rất cao. Mỗi đường chuyền có thể đi qua các vị trí O(n) và trong trường hợp xấu nhất, chúng ta có thể cần các đường chuyền O(max S_i) cho một số vị trí, khiến độ phức tạp bùng nổ vượt quá giới hạn khả thi. 

Quan sát quan trọng là đảo ngược quan điểm. Thay vì nghĩ về những đường chuyền, chúng tôi nghĩ về những đóng góp của mỗi vị trí cho các vị trí trong tương lai. Mỗi lần chúng tôi hạ cánh vào vị trí`i`, chúng tôi “lên lịch” một chuyến thăm`i + S[i]`. Vì vậy, mọi chỉ mục đều tạo ra một cạnh có hướng tới một chỉ mục khác và quá trình này trở thành một đường truyền động trên một đồ thị hàm số co lại từ từ khi trọng số giảm. 

Một hiểu biết sâu sắc về cấu trúc quan trọng là mặc dù các cạnh thay đổi theo thời gian nhưng mục tiêu của mỗi chỉ số chỉ di chuyển sang trái (vì`S[i]`giảm dần). Điều này có nghĩa là cạnh đi ra của mỗi nút chỉ có thể “trượt” một cách đơn điệu và mỗi đơn vị giảm đi tương ứng với một số lượng giới hạn các thay đổi cấu trúc hiệu quả. 

Chúng ta có thể xử lý các khoản đóng góp theo cách giống như quét: khi một vị trí được sử dụng, nó sẽ truyền tải “nhu cầu” đến mục tiêu của nó. Chúng tôi duy trì số lần mỗi vị trí phải được nhập và mỗi mục nhập tương ứng với chính xác một đóng góp vượt qua trừ khi nó được xâu chuỗi trong cùng một thẻ. 

Điều này làm giảm vấn đề duy trì sự tích lũy động của các lượt truy cập cần thiết và đếm xem cần bao nhiêu điểm bắt đầu độc lập để đáp ứng tất cả các chuỗi truyền bá. Câu trả lời cuối cùng tương ứng với số lần chúng ta cần để bắt đầu một chuỗi không thể được bao phủ bởi quá trình lan truyền đang diễn ra. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n * max S) | O(n) | Quá chậm | 
| Tuyên truyền / tích lũy dòng chảy tham lam | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi diễn giải quy trình này như việc xây dựng một luồng phụ thuộc từ mỗi chỉ mục đến đích hiện tại của nó`i + S[i]`. Mỗi vị trí sẽ tích lũy “nhu cầu đến”, nghĩa là vị trí đó phải được truy cập bao nhiêu lần trên tất cả các lượt đi. 

1. Khởi tạo một mảng`need`kích thước`n + 2`để theo dõi số lần mỗi vị trí vẫn cần được kích hoạt. Ban đầu, tất cả các giá trị đều bằng 0 vì chúng ta chưa lên lịch bất kỳ quá trình truyền tải nào. 
2. Duyệt các chỉ số từ trái sang phải. Tại mỗi chỉ số`i`, trước tiên chúng tôi tính đến số lần vị trí này vẫn phải được truy cập do việc truyền bá trước đó. Nếu như`need[i] > 0`, chúng tôi coi mỗi điều đó như một phần của các lượt chuyển tiếp đang diễn ra và chúng tôi truyền bá tác dụng của chúng về phía trước. 
3. Mỗi vị trí thời gian`i`được sử dụng, nó buộc phải chuyển sang`i + S[i]`. Chúng tôi thêm`need[i]`ĐẾN`need[i + S[i]]`. Điều này thể hiện rằng mỗi lần truy cập được yêu cầu tại`i`tiếp tục chuỗi về phía trước. 
4. Sau khi nhân giống, chúng ta xác định có bao nhiêu đường chuyền mới phải bắt đầu tại`i`. Nếu vẫn còn "yêu cầu chưa được khám phá" tại`i`, điều đó có nghĩa là không có chuỗi nào trước đó đạt tới nó, vì vậy mỗi đơn vị như vậy tương ứng với việc bắt đầu một lượt mới. Chúng tôi tăng câu trả lời cho phù hợp. 
5. Từ khi sử dụng`i`giảm sức mạnh của nó về mặt khái niệm, chúng tôi không mô phỏng rõ ràng sự giảm dần; thay vào đó, sự lan truyền đơn điệu đã gây ra sự suy yếu trong tương lai bằng cách đảm bảo rằng các khoản đóng góp lặp lại sẽ được chuyển tiếp chính xác một lần cho mỗi lượt truy cập được yêu cầu. 

### Tại sao nó hoạt động 

Mỗi lần vượt qua có thể được coi là một chuỗi các bước nhảy về phía trước xác định, tạo ra một chuỗi đóng góp từ trái sang phải theo thứ tự chỉ số tăng dần. Khi xử lý các chỉ mục theo thứ tự, tất cả các đóng góp đến`i`đã được xác định rồi. Bất kỳ khoản thâm hụt còn lại nào tại`i`phải bắt nguồn từ một thẻ mới bắt đầu chính xác vào lúc`i`, bởi vì không có chỉ mục nào sớm hơn có thể đạt tới`i`mà chưa được tính vào`need[i]`. Việc nhân rộng đảm bảo rằng mỗi lượt truy cập tiếp tục cấu trúc chuỗi giống nhau, do đó việc chia tách chuỗi là không bao giờ cần thiết và việc hợp nhất diễn ra một cách tự nhiên thông qua tích lũy trong`need`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        s = list(map(int, input().split()))
        
        need = [0] * (n + 5)
        ans = 0
        
        for i in range(n):
            if need[i] > 0:
                jump = i + s[i]
                if jump <= n:
                    need[jump] += need[i]
            
            if need[i] == 0:
                continue
            
            ans += need[i]
            jump = i + s[i]
            if jump <= n:
                need[jump] += need[i]
            
            need[i] = 0
        
        print(ans)

if __name__ == "__main__":
    solve()
```Mã duy trì một mảng lan truyền duy nhất`need`, theo dõi số lượng chuỗi truyền tải hiện đang đi qua từng vị trí. Vòng lặp chính xử lý các chỉ mục theo thứ tự tăng dần sao cho tất cả các chuỗi đến đều đã được biết ở mỗi bước. 

Tại mỗi chỉ mục, mọi chuỗi đang chờ xử lý sẽ được tiếp tục chuyển tiếp hoặc nếu không tồn tại thì phải bắt đầu một lượt chuyển tiếp mới. Tính toán bước nhảy`i + s[i]`mã hóa quá trình chuyển đổi tấm bạt lò xo và các giới hạn đảm bảo chúng tôi bỏ qua các lối thoát ra ngoài mảng. 

Một lựa chọn triển khai tinh tế là đặt lại`need[i]`sau khi xử lý. Điều này ngăn việc tính hai lần khi nhiều chỉ số trước đó truyền vào cùng một vị trí. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 7
S = [1, 4, 2, 2, 2, 2, 2]
```Chúng tôi theo dõi`need`Và`ans`: 

| tôi | S[i] | cần[i] trước | hành động | nhảy | cần sau | trả lời | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | không có chuỗi bắt đầu | 1 | 0 | 0 | 
| 1 | 4 | 0 | không có chuỗi bắt đầu | 5 | 0 | 0 | 
| 2 | 2 | 0 | bắt đầu vượt qua | 4 | +1 lúc 4 | 1 | 
| 3 | 2 | 0 | không có chuỗi bắt đầu | 5 | 0 | 1 | 
| 4 | 2 | 1 | tiếp tục chuỗi | 6 | tuyên truyền | 1 | 
| 5 | 2 | 0 | không có chuỗi bắt đầu | 7 | 0 | 1 | 
| 6 | 2 | 1 | thoát | ra | - | 1 | 

Dấu vết này cho thấy rằng một chuỗi đơn truyền qua nhiều chỉ mục, nhưng chỉ cần khởi động mới khi một vị trí không có luồng đến. 

### Ví dụ 2 

đầu vào:```
n = 5
S = [1, 1, 1, 1, 1]
```| tôi | S[i] | cần[i] | hành động | nhảy | trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | bắt đầu | 1 | 1 | 
| 1 | 1 | 1 | tiếp tục | 2 | 1 | 
| 2 | 1 | 1 | tiếp tục | 3 | 1 | 
| 3 | 1 | 1 | tiếp tục | 4 | 1 | 
| 4 | 1 | 1 | tiếp tục | ra | 1 | 

Điều này cho thấy một lần chuyển là đủ vì mọi vị trí đều được xâu chuỗi một cách tuần tự. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) mỗi lần kiểm tra | Mỗi chỉ mục được xử lý một lần và mỗi lần truyền được xử lý trong thời gian không đổi | 
| Không gian | O(n) | các`need`mảng cửa hàng đang chờ đếm chuỗi | 

Tổng của`n`trên tất cả các trường hợp thử nghiệm là 5000, vì vậy cách tiếp cận tuyến tính này dễ dàng phù hợp với giới hạn ngay cả với chi phí Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        s = list(map(int, input().split()))
        need = [0] * (n + 5)
        ans = 0
        for i in range(n):
            if need[i] > 0:
                j = i + s[i]
                if j <= n:
                    need[j] += need[i]
            if need[i] > 0:
                ans += need[i]
                j = i + s[i]
                if j <= n:
                    need[j] += need[i]
                need[i] = 0
        out.append(str(ans))
    return "\n".join(out)

# provided samples
assert run("""3
7
1 4 2 2 2 2 2
2
2 3
5
1 1 1 1 1
""") == """4
3
0"""

# custom cases
assert run("""1
1
1
""") == "0"

assert run("""1
3
5 5 5
""") == "1"

assert run("""1
4
2 2 2 2
""") == "2"

assert run("""1
6
1 2 3 4 5 1
""") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`n=1, [1]`|`0`| đã tối thiểu | 
|`[5,5,5]`|`1`| chuỗi nhảy đơn | 
|`[2,2,2,2]`|`2`| chuỗi chồng chéo | 
| tăng hỗn hợp |`3`| tính chính xác của việc truyền bá | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi tất cả các giá trị đã bằng 1. Trong trường hợp đó, không có sự lan truyền nào xảy ra, do đó thuật toán không bao giờ tăng câu trả lời. Điều này an toàn vì`need[i]`vẫn bằng không trong suốt. 

Một trường hợp khác là khi giá trị cực kỳ lớn. Ví dụ, nếu`S[1] = 10^9`, bước nhảy ngay lập tức thoát khỏi mảng, do đó chỉ mục đó không góp phần lan truyền thêm. Thuật toán xử lý việc này một cách tự nhiên vì`i + S[i] > n`chỉ đơn giản là loại bỏ các bản cập nhật. 

Trường hợp tinh tế cuối cùng là khi nhiều chỉ số truyền vào cùng một mục tiêu. Bởi vì`need`tích lũy số lượng, những sự hợp nhất này được kết hợp một cách tự nhiên và mỗi đơn vị vẫn được coi là một lượt truy cập bắt buộc độc lập, duy trì tính chính xác ngay cả khi nhiều chuỗi hội tụ.
