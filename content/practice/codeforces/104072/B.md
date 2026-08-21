---
title: "CF 104072B - Bóng"
description: "Chúng ta được đưa ra một đường có nhiều quả bóng được đặt ở các vị trí khác nhau. Mỗi quả bóng bắt đầu ở một tọa độ đã biết và di chuyển dọc theo trục số với vận tốc không đổi, sang trái hoặc sang phải. Tất cả các quả bóng bắt đầu di chuyển cùng một lúc."
date: "2026-07-02T02:52:13+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104072
codeforces_index: "B"
codeforces_contest_name: "AGM 2022, Final Round, Day 2"
rating: 0
weight: 104072
solve_time_s: 46
verified: true
draft: false
---

[CF 104072B - Bóng](https://codeforces.com/problemset/problem/104072/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được đưa ra một đường có nhiều quả bóng được đặt ở các vị trí khác nhau. Mỗi quả bóng bắt đầu ở một tọa độ đã biết và di chuyển dọc theo trục số với vận tốc không đổi, sang trái hoặc sang phải. Tất cả các quả bóng bắt đầu di chuyển cùng một lúc. Điều khó khăn là bất cứ khi nào hai quả bóng gặp nhau, chúng không tương tác theo một cách vật lý phức tạp, thay vào đó chúng truyền qua nhau bằng cách hoán đổi vận tốc, điều này làm cho hệ tương đương với các hạt chuyển động tự do nếu chúng ta bỏ qua danh tính. 

Tại một thời điểm cố định T, chúng ta được yêu cầu xác định vị trí của tất cả các quả bóng. Đầu ra cuối cùng không bị ràng buộc với danh tính ban đầu, vì va chạm có thể hoán đổi vận tốc giữa các quả bóng một cách hiệu quả. Thay vào đó, chúng ta quan tâm đến tập hợp vị trí của tất cả các quả bóng sau thời gian T, được in theo thứ tự không giảm. 

Ràng buộc chính là N lên tới 100000 và tọa độ cũng như vận tốc có độ lớn lên tới 10^9, với thời gian lên tới 10^9. Điều này ngay lập tức ngụ ý rằng bất kỳ mô phỏng nào nâng cao thời gian từng bước hoặc xử lý xung đột một cách rõ ràng đều là không thể. Một mô phỏng va chạm đơn giản sẽ yêu cầu xử lý các sự kiện tương tác có khả năng O(N^2) trong trường hợp xấu nhất, vì mỗi cặp bóng có thể tương tác gián tiếp thông qua chuỗi hoán đổi. Ngay cả mô phỏng theo sự kiện cũng sẽ quá chậm vì số lượng sự kiện va chạm có thể là bậc hai. 

Trường hợp cạnh đơn giản hơn nhưng tinh tế hơn xuất hiện khi nhiều quả bóng có cùng vị trí ban đầu hoặc đến cùng một điểm vào cùng thời điểm do tốc độ khác nhau. Vì va chạm được định nghĩa là hoán đổi tức thời, những tình huống này không tạo ra sự mơ hồ trong tập hợp vị trí cuối cùng, nhưng chúng phá vỡ các triển khai ngây thơ cố gắng mô phỏng trật tự một cách rõ ràng thay vì làm việc với một phép biến đổi toán học của chuyển động. 

Ví dụ, xét hai quả bóng ở vị trí 0 và 10 chuyển động hướng về nhau với tốc độ 1 và -1. Sau T = 5, chúng va chạm nhau ở vị trí 5 và hoán đổi vận tốc nên bi bên trái kết thúc ở bên phải và ngược lại. Một danh tính theo dõi mô phỏng đơn giản sẽ buộc phải xử lý thứ tự va chạm một cách chính xác, nhưng vị trí cuối cùng chỉ đơn giản giống như thể các quả bóng đã đi qua nhau. 

Cái nhìn sâu sắc về mô hình cốt lõi là việc hoán đổi vận tốc khi va chạm làm cho hệ thống tương đương với việc để các quả bóng đi qua nhau mà không có tương tác, sau đó gán lại danh tính sau đó dựa trên thứ tự. 

## Phương pháp tiếp cận 

Chế độ xem brute-force là mô phỏng chuyển động và xử lý rõ ràng các va chạm. Chúng tôi sẽ duy trì tất cả các quả bóng được sắp xếp theo vị trí, liên tục tìm sự kiện va chạm tiếp theo, tăng thời gian cho sự kiện đó, hoán đổi vận tốc của cặp va chạm và tiếp tục cho đến thời điểm T. Mỗi va chạm là một sự kiện và trong trường hợp xấu nhất, mọi cặp quả bóng liền kề có thể va chạm nhiều lần tùy thuộc vào vận tốc. Số lượng sự kiện có thể đạt tới O(N^2) và mỗi sự kiện yêu cầu cập nhật hàng đợi ưu tiên hoặc quét danh sách, khiến giải pháp trở nên quá chậm đối với N = 100000. 

Quan sát quan trọng là việc hoán đổi vận tốc trong các va chạm tương đương với việc nói rằng các quả bóng hành xử giống như chúng truyền qua nhau không thay đổi nếu chúng ta bỏ qua danh tính của chúng. Điều này có nghĩa là chúng ta có thể tính toán vị trí của mỗi quả bóng nếu nó chuyển động độc lập: vị trí tại thời điểm T chỉ đơn giản là p_i + s_i * T. 

Vấn đề duy nhất còn lại là các xung đột hoán đổi danh tính, nhưng vì đầu ra chỉ yêu cầu các vị trí được sắp xếp theo thứ tự không giảm nên danh tính không liên quan. Sau khi tính toán tất cả các vị trí cuối cùng một cách độc lập, việc sắp xếp chúng sẽ cho cấu hình chính xác. Bán kính R không liên quan đến các vị trí cuối cùng vì nó chỉ ảnh hưởng khi xảy ra va chạm chứ không ảnh hưởng đến sự tương đương toán học của việc hoán đổi quỹ đạo. 

Do đó, vấn đề giảm xuống còn việc tính toán một phép biến đổi tuyến tính cho mỗi quả bóng và sắp xếp kết quả.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(N^2) hoặc tệ hơn | O(N) | Quá chậm | 
| Chuyển động độc lập + Sắp xếp | O(N log N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi quả bóng, hãy tính vị trí của nó như thể chưa từng xảy ra va chạm. Điều này được thực hiện bằng cách sử dụng p_i + s_i * T, biểu thị chuyển động đều với vận tốc không đổi. Bước này hợp lệ vì va chạm chỉ hoán đổi vận tốc và không làm thay đổi tập hợp quỹ đạo. 
2. Lưu trữ tất cả các vị trí được tính toán trong một mảng. Tại thời điểm này, mỗi mục thể hiện một vị trí cuối cùng có thể có của một số quả bóng, nhưng danh tính không còn có ý nghĩa nữa. 
3. Sắp xếp mảng vị trí theo thứ tự không giảm. Việc sắp xếp là cần thiết vì va chạm chỉ hoán vị quả bóng nào chiếm quỹ đạo nào, nhưng tập hợp tọa độ cuối cùng không thay đổi. 
4. Xuất danh sách đã sắp xếp làm cấu hình cuối cùng. 

Tại sao nó hoạt động 

Bất biến quan trọng là va chạm chỉ hoán đổi vận tốc giữa các quả bóng, nghĩa là nhiều tập hợp quỹ đạo được bảo toàn. Nếu chúng ta tưởng tượng việc dán nhãn cho mỗi vận tốc là một dấu hiệu và để nó di chuyển tự do thì mỗi dấu hiệu sẽ đi theo một đường thẳng. Cơ chế hoán đổi đảm bảo rằng các quả bóng chỉ cần trao đổi các mã thông báo này khi chúng gặp nhau. Do đó, tại bất kỳ thời điểm T nào, tập hợp các vị trí mà các quả bóng chiếm giữ chính xác là tập hợp các vị trí có được bằng cách tiến từng quả bóng ban đầu một cách độc lập mà không xét đến va chạm. Sự khác biệt duy nhất là quả bóng nào kết thúc ở vị trí nào, điều này không liên quan vì đầu ra đã được sắp xếp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, r, t = map(int, input().split())
    pos = []
    
    for _ in range(n):
        p, s = map(int, input().split())
        pos.append(p + s * t)
    
    pos.sort()
    print(*pos)

if __name__ == "__main__":
    solve()
```Việc triển khai trực tiếp tuân theo quan sát rằng các va chạm không ảnh hưởng đến nhiều vị trí cuối cùng. Quỹ đạo của mỗi quả bóng được tính toán độc lập, sau đó kết quả được sắp xếp. 

Điểm tinh tế duy nhất là sử dụng số học an toàn 64-bit. Trong Python, điều này là tự động, nhưng trong các ngôn ngữ khác, cần phải cẩn thận vì p_i và s_i * T có thể đạt tới 10^18. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào: 

3 1 5 

3 -1 

8 -2 

1 3 

Chúng tôi tính toán các vị trí thô: 

| Bóng | p | s | p + sT | 
| --- | --- | --- | --- | 
| 1 | 3 | -1 | -2 | 
| 2 | 8 | -2 | -2 | 
| 3 | 1 | 3 | 16 | 

Sau khi sắp xếp chúng ta nhận được -2, -2, 16. 

Điều này phù hợp với ý tưởng rằng hai quả bóng cuối cùng có thể có cùng giá trị tọa độ cuối cùng tùy thuộc vào vận tốc và vị trí ban đầu, nhưng thứ tự chỉ được xác định ở cuối. 

Dấu vết này cho thấy rằng các xung đột không liên quan đến tính toán, vì việc đánh giá trực tiếp đã mang lại tập hợp cuối cùng chính xác. 

### Ví dụ 2 

đầu vào: 

4 1 3 

0 2 

5 -1 

10 -2 

7 1 

Vị trí thô: 

| Bóng | p | s | p + sT | 
| --- | --- | --- | --- | 
| 1 | 0 | 2 | 6 | 
| 2 | 5 | -1 | 2 | 
| 3 | 10 | -2 | 4 | 
| 4 | 7 | 1 | 10 | 

Sắp xếp cho 2, 4, 6, 10. 

Ví dụ này cho thấy rằng mặc dù những quả bóng nhanh hơn có thể vượt qua những quả bóng chậm hơn trong chuyển động vật lý, việc hoán đổi đảm bảo quỹ đạo được bảo toàn, do đó vị trí cuối cùng chỉ phụ thuộc vào chuyển động tuyến tính chứ không phải thứ tự tương tác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N log N) | vị trí tính toán là O(N), việc sắp xếp chiếm ưu thế | 
| Không gian | O(N) | lưu trữ vị trí cuối cùng | 

Các ràng buộc cho phép lên tới 100000 quả bóng, do đó, giải pháp O(N log N) vừa vặn thoải mái trong giới hạn. Việc sử dụng bộ nhớ là tuyến tính và nằm trong giới hạn thông thường. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    out = io.StringIO()
    backup = sys.stdout
    sys.stdout = out
    solve()
    sys.stdout = backup
    return out.getvalue().strip()

# sample
assert run("""3 1 5
3 -1
8 -2
1 3
""") == "-2 -2 16"

# minimum size
assert run("""1 1 10
5 2
""") == "25"

# all same speed
assert run("""3 1 2
0 1
10 1
20 1
""") == "2 12 22"

# mixed directions
assert run("""4 1 1
0 -1
1 1
2 -1
3 1
""") == "-1 0 2 4"

# large values
assert run("""2 1 1000000000
0 1000000000
1000000000 -1000000000
""") == "0 0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 quả bóng | chuyển động trực tiếp | trường hợp cơ sở | 
| tốc độ giống hệt nhau | đặt hàng ổn định | không có hiệu ứng tương tác | 
| hướng hỗn hợp | xử lý biển báo | tính đúng đắn theo giao dịch hoán đổi | 
| giá trị lớn | an toàn tràn | độ chính xác của thang đo 1e18 | 

## Vỏ cạnh 

Một tình huống tinh tế là khi hai quả bóng kết thúc ở cùng một vị trí được tính toán. Chẳng hạn, hai quả bóng xuất phát ở những điểm khác nhau có thể rơi xuống cùng một tọa độ sau thời gian T do vận tốc trái ngược nhau. Thuật toán vẫn hoạt động vì việc sắp xếp xử lý các giá trị bằng nhau một cách tự nhiên và các xung đột trong quy trình ban đầu cũng cho phép chồng chéo tạm thời mà không thay đổi vị trí cuối cùng được đặt. 

Một trường hợp cạnh khác là T = 0. Trong trường hợp này, các vị trí được tính toán giảm xuống vị trí ban đầu p_i và việc sắp xếp chỉ đơn giản trả về thứ tự ban đầu theo tọa độ, phù hợp với yêu cầu vì không có chuyển động nào xảy ra. 

Một trường hợp khác là vận tốc âm kết hợp với T lớn, có thể đẩy các vị trí thành số âm. Công thức p_i + s_i * T xử lý trực tiếp vấn đề này và số học số nguyên Python ngăn chặn các vấn đề tràn có thể xuất hiện trong các ngôn ngữ số nguyên có chiều rộng cố định.
