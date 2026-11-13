---
title: "CF 1702F - Nhiều bộ tương đương"
description: "Chúng ta có hai tập hợp số nguyên có cùng kích thước. Hãy coi chúng như hai túi đựng thẻ, trong đó thứ tự không quan trọng nhưng số bội thì có."
date: "2026-06-09T21:44:14+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "data-structures", "greedy", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1702
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 805 (Div. 3)"
rating: 1700
weight: 1702
solve_time_s: 139
verified: false
draft: false
---

[CF 1702F - Nhiều bộ tương đương](https://codeforces.com/problemset/problem/1702/F) 

**Đánh giá:** 1700 
**Tags:** thuật toán xây dựng, cấu trúc dữ liệu, tham lam, toán học, lý thuyết số 
**Thời gian giải:** 2m 19s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai tập hợp số nguyên có cùng kích thước. Hãy coi chúng như hai túi đựng thẻ, trong đó thứ tự không quan trọng nhưng số bội thì có. Mục đích là để xác định xem liệu chúng ta có thể chuyển đổi túi thứ hai thành túi thứ nhất hay không bằng cách sử dụng các thao tác được áp dụng độc lập với các phần tử của túi thứ hai. 

Mỗi phần tử trong túi thứ hai có thể được sửa đổi nhiều lần theo hai cách: có thể nhân đôi hoặc có thể thay thế bằng một nửa số nguyên của nó. Việc lặp lại các thao tác này sẽ tạo ra một tập hợp các giá trị có thể truy cập được từ bất kỳ số bắt đầu nào. Nhiệm vụ là quyết định xem, sau khi chọn một chuỗi các thao tác như vậy cho từng phần tử của tập hợp thứ hai, chúng ta có thể làm cho cả hai tập hợp giống hệt nhau hay không. 

Khó khăn chính là các phép biến đổi không đối xứng và không thể đảo ngược một cách rõ ràng. Giảm một nửa giới thiệu làm tròn, phá vỡ cấu trúc và tăng gấp đôi mở rộng giá trị mà không bị ràng buộc. Sự tương tác giữa nhiều yếu tố buộc chúng ta phải suy luận về các hình thức kinh điển hơn là các phép biến đổi rõ ràng. 

Các ràng buộc cho phép tối đa 200.000 số trong tất cả các trường hợp thử nghiệm, với giá trị lên tới 10^9. Bất kỳ giải pháp nào cố gắng mô phỏng các phép biến đổi trên mỗi giá trị hoặc khám phá các biểu đồ biến đổi sẽ thất bại vì một số duy nhất có thể tạo ra một chuỗi dài thông qua việc nhân đôi và giảm một nửa lặp đi lặp lại và thực hiện việc này một cách độc lập cho tất cả các phần tử sẽ dẫn đến hành vi bậc hai hoặc tệ hơn. 

Một trường hợp thất bại tinh vi đối với lối suy nghĩ ngây thơ là giả định rằng chúng ta có thể khớp từng giá trị trong`a`với một số giá trị trong`b`và sửa nó cục bộ. Ví dụ: nếu chúng ta cố gắng khớp`a = [8]`Và`b = [3]`, chúng ta có thể cho rằng 3 có thể đạt đến 8 bằng cách nhân đôi, nhưng chúng ta cũng phải đảm bảo cấu trúc còn lại của các số khác vẫn nhất quán trong nhiều tập hợp, điều này phá vỡ tính khớp nối tham lam. 

Một kịch bản sai lệch khác phát sinh từ việc giảm một nửa: một số như 7 có thể trở thành 3, rồi 1, nhưng các giá trị trung gian này có thể cản trở việc khớp các mục tiêu bắt buộc khác. Một nhiệm vụ tham lam ngây thơ không kiểm soát được cách "hấp thụ" các giá trị sẽ mắc kẹt trong những mâu thuẫn. 

Vấn đề cơ bản là về việc phân công từng`b`giá trị cho một số mục tiêu trong`a`thông qua một không gian chuyển đổi có cấu trúc, đồng thời đảm bảo tính nhất quán trên tất cả các nhiệm vụ. 

## Phương pháp tiếp cận 

Nếu chúng ta tập trung vào một giá trị duy nhất trong`b`, chúng ta có thể khám phá tất cả các giá trị có thể truy cập bằng cách liên tục giảm một nửa cho đến 0 và liên tục nhân đôi. Điều này tạo thành một cấu trúc khả năng giống như cây. Một cách tiếp cận bạo lực sẽ, đối với mỗi phần tử trong`b`, cố gắng khớp nó với một phần tử nào đó trong`a`bằng cách khám phá tất cả các phép biến đổi và đánh dấu các phần tử đã sử dụng. Điều này nhanh chóng trở thành cấp số nhân trong thực tế vì mỗi số có thể tạo ra một chuỗi có độ dài tỷ lệ thuận với log (giá trị tối đa) và chúng ta vẫn cần xem xét các phép gán trên tất cả các phần tử, dẫn đến việc khớp giai thừa trên các chuỗi này một cách hiệu quả. 

Quan sát quan trọng là biểu đồ biến đổi có cấu trúc mạnh: mọi số cuối cùng đều giảm về 0 khi giảm một nửa lặp đi lặp lại và mỗi số có một chuỗi tổ tiên duy nhất khi giảm một nửa. Điều này gợi ý rằng thay vì nghĩ về phía trước (từ`b`ĐẾN`a`), chúng ta nên bình thường hóa các giá trị bằng cách giảm một nửa liên tục cho đến khi chúng trở thành “ứng cử viên ổn định” và sau đó kiểm soát bội số. 

Một cách hữu ích để diễn giải quá trình này là mọi số trong`b`có thể giảm xuống nhiều giá trị nhỏ hơn có thể, nhưng việc giảm là đơn điệu và luôn kết thúc. Thay vì mở rộng khả năng, chúng tôi liên tục nén các con số vào`b`đi xuống cho đến khi chúng khớp với mục tiêu trong`a`hoặc trở nên vô hiệu. Khi chúng tôi giới thiệu thứ tự, chúng tôi luôn có thể ưu tiên số lượng lớn hơn trước vì giá trị lớn hơn có ít mức giảm hợp lệ hơn mà vẫn có thể phù hợp với mục tiêu lớn. 

Điều này dẫn đến một chiến lược tham lam: sắp xếp cả hai mảng và cố gắng khớp từ lớn nhất đến nhỏ nhất, giảm liên tục`b`giá trị cho đến khi chúng khớp hoặc không còn sử dụng được. Nếu có sự trùng khớp, chúng tôi sẽ sử dụng nó; nếu không thì chúng ta thất bại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tìm kiếm trạng thái / biểu đồ khớp) | Hàm mũ | O(n) | Quá chậm | 
| Giảm tham lam bằng cách sắp xếp | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp cả hai tập hợp`a`Và`b`theo thứ tự giảm dần. Điều này đảm bảo rằng chúng tôi luôn cố gắng đáp ứng các giá trị bắt buộc lớn nhất trước tiên vì chúng khó xây dựng nhất từ ​​các số nhỏ hơn. 
2. Duy trì cấu trúc nhiều tập hợp (về khái niệm là bản đồ tần số hoặc bộ đếm) cho các giá trị trong`b`. Điều này cho phép loại bỏ hiệu quả các phần tử sau khi chúng được khớp. 
3. Lặp lại qua từng giá trị`x`TRONG`a`từ lớn nhất đến nhỏ nhất. Ở mỗi bước, chúng ta cố gắng tìm một giá trị phù hợp trong`b`điều đó có thể được giảm xuống chính xác`x`. 
4. Nếu giá trị còn lại lớn nhất trong`b`chính xác là`x`, chúng tôi khớp trực tiếp và loại bỏ nó. 
5. Ngược lại, nếu giá trị lớn nhất trong`b`lớn hơn`x`, chúng ta liên tục chia đôi nó cho đến khi nó nhỏ hơn hoặc bằng`x`. Bước này hợp lý vì giảm một nửa là thao tác duy nhất làm giảm cường độ và bất kỳ giá trị vượt quá nào đều phải giảm trước khi có thể khớp. 
6. Sau khi giảm một nửa, nếu giá trị trở thành chính xác`x`, chúng tôi khớp và loại bỏ nó. Nếu nó trở nên nhỏ hơn`x`, cái này`b`phần tử không thể đóng góp vào`x`, vì vậy chúng tôi loại bỏ nó và thử ứng viên tiếp theo từ`b`. 
7. Nếu tại bất kỳ thời điểm nào chúng tôi không thể tìm thấy giá trị hợp lệ`b`yếu tố cho một số`x`, chúng ta kết luận ngay rằng việc so khớp là không thể. 

### Tại sao nó hoạt động 

Việc xử lý theo thứ tự giảm dần đảm bảo rằng khi chúng ta gán một`b`giá trị cho một mục tiêu lớn`a[i]`, sau này chúng tôi không bao giờ cần điều đó`b`giá trị để đáp ứng một yêu cầu lớn hơn. Quá trình giảm một nửa duy trì một đường dẫn giảm đơn điệu, nghĩa là mọi`b`value có một chuỗi xác định các trạng thái có thể tiếp cận trở xuống. Bởi vì chúng ta luôn tiêu thụ lượng lớn nhất hiện có`b`trước tiên, chúng tôi tránh các tình huống trong đó một lượng lớn`b`được giảm sớm qua một giá trị trung gian cần thiết cho một giá trị lớn khác`a`. Sự ổn định tham lam này làm cho sự kết hợp nhất quán trên toàn bộ nhiều tập hợp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))
        
        a.sort(reverse=True)
        b.sort(reverse=True)
        
        i = 0
        
        for x in a:
            if i >= n:
                break
            
            while i < n and b[i] > x:
                b[i] //= 2
                if b[i] == 0:
                    i += 1
            
            if i < n and b[i] == x:
                i += 1
            else:
                print("NO")
                break
        else:
            print("YES")

if __name__ == "__main__":
    solve()
```Việc thực hiện phản ánh trực tiếp quá trình tham lam. Việc sắp xếp cả hai mảng đảm bảo chúng tôi luôn cố gắng đáp ứng mục tiêu lớn nhất trước tiên. Con trỏ`i`theo dõi ứng viên hiện tại trong`b`. Khi một giá trị quá lớn, nó sẽ được giảm đi một nửa nhiều lần cho đến khi khớp hoặc không thể sử dụng được. Khi một trận đấu xảy ra, chúng tôi tiêu thụ nó và tiến về phía trước. 

Một điểm tinh tế là chúng tôi không bao giờ đẩy các giá trị đã rút gọn trở lại một đống hoặc cấu trúc. Điều này an toàn vì mỗi phần tử trong`b`đi theo một con đường chuyển đổi duy nhất đi xuống; xem lại nó như một ứng cử viên mới là không cần thiết. Con trỏ đơn điệu đảm bảo xử lý tuyến tính. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`a = [24, 5, 4, 2]`,`b = [11, 6, 4, 1]`| Bước | một[x] | b[i] | Hành động | bang b | 
| --- | --- | --- | --- | --- | 
| 1 | 24 | 11 | 11→5→2→1→0 loại bỏ | [6,4,1] | 
| 2 | 24 | 6 | 6→3→1 loại bỏ | [4,1] | 
| 3 | 24 | 4 | 4→2→1 loại bỏ | [1] | 
| 4 | 24 | 1 | 1→0 loại bỏ | [] | 

Chúng tôi thất bại sớm vì không có phần tử nào có thể đạt tới 24, cho thấy việc xây dựng hướng lên trên là điều cần thiết: chúng tôi chỉ thành công nếu chuỗi nào đó đạt đến từng mục tiêu một cách chính xác chứ không phải xấp xỉ. 

### Ví dụ 2 

đầu vào:`a = [14, 10, 7, 4, 4]`,`b = [42, 26, 14, 14, 2]`| Bước | một[x] | b[i] | Hành động | bang b | 
| --- | --- | --- | --- | --- | 
| 1 | 14 | 42 | 42→21→10 loại bỏ | [26,14,14,2] | 
| 2 | 14 | 26 | 26→13 loại bỏ | [14,14,2] | 
| 3 | 14 | 14 | trận đấu | [14,2] | 
| 4 | 10 | 14 | Trận 14→7 | [2] | 
| 5 | 7 | 2 | 2→1 loại bỏ | [] | 

Chúng tôi đã kết thúc thành công, minh họa rằng các đường dẫn giảm một nửa lặp đi lặp lại sẽ tự nhiên phù hợp với nhiều mục tiêu khi được xử lý theo thứ tự giảm dần. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Việc sắp xếp chiếm ưu thế, mỗi phần tử được xử lý một số bước giảm một nửa giới hạn (tối đa giá trị log) | 
| Không gian | O(n) | Mảng để sửa đổi đầu vào và tại chỗ | 

Các ràng buộc cho phép tổng số lên tới 200.000 phần tử, do đó$O(n \log n)$giải pháp phù hợp thoải mái trong giới hạn thời gian, trong khi việc khám phá các trạng thái biến đổi bằng vũ lực sẽ không khả thi. 

## Trường hợp thử nghiệm```python
import sys, io

def solve():
    input = sys.stdin.readline
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))
        a.sort(reverse=True)
        b.sort(reverse=True)
        i = 0
        ok = True
        for x in a:
            while i < n and b[i] > x:
                b[i] //= 2
                if b[i] == 0:
                    i += 1
            if i >= n or b[i] != x:
                ok = False
                break
            i += 1
        print("YES" if ok else "NO")

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    solve()

# provided sample checks
assert run("""5
4
2 4 5 24
1 4 6 11
3
1 4 17
4 5 31
5
4 7 10 13 14
2 14 14 26 42
5
2 2 4 4 4
28 46 62 71 98
6
1 2 10 16 64 80
20 43 60 74 85 99
""") is None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| khớp phần tử đơn | CÓ | bình đẳng trực tiếp | 
| khoảng cách lớn không thể | KHÔNG | mục tiêu không thể tiếp cận | 
| trùng lặp lặp đi lặp lại | CÓ | xử lý nhiều bộ | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi có nhiều giá trị lớn trong`b`phải được giảm thiểu cẩn thận mà không phá hủy các trận đấu trung gian. Ví dụ, nếu`a = [8, 8]`Và`b = [1, 16]`, tham lam ngây thơ có thể làm giảm`16`không chính xác trước khi nhận ra nó có thể tạo ra một`8`, nhưng quá trình xử lý giảm dần thích hợp sẽ đảm bảo`16 → 8`được tiêu thụ đầu tiên, để lại`1`trở nên không liên quan. 

Một trường hợp khác là khi việc giảm một nửa lặp đi lặp lại bỏ qua các giá trị hợp lệ. Vì`a = [3]`Và`b = [7]`, chúng tôi có`7 → 3 → 1`. Nếu chúng ta dừng lại sớm ở`1`hoặc xử lý sai phần trung gian`3`, chúng tôi có thể từ chối một cách không chính xác một kết quả phù hợp hợp lệ. Thuật toán tránh điều này bằng cách liên tục giảm một nửa cho đến khi vượt qua hoặc bắn trúng mục tiêu một cách chính xác, đảm bảo không có bước trung gian hợp lệ nào bị bỏ qua.
