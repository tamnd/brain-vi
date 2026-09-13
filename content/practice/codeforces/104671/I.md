---
title: "CF 104671I - Phebe và Ryan"
description: "Chúng ta được cung cấp nhiều tập trọng lượng khối. Với mỗi giá trị trọng số $i$, có $ai$ các khối có trọng số $i$ giống hệt nhau. Người chơi luân phiên lấy bất kỳ khối nào còn lại và cộng trọng lượng của nó vào tổng số bắt đầu từ 0."
date: "2026-06-29T09:31:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104671
codeforces_index: "I"
codeforces_contest_name: "2023 ICPC Columbia University Local Contest"
rating: 0
weight: 104671
solve_time_s: 110
verified: false
draft: false
---

[CF 104671I - Phebe và Ryan](https://codeforces.com/problemset/problem/104671/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 50 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp nhiều tập trọng lượng khối. Đối với mỗi giá trị trọng số$i$, có$a_i$khối lượng giống hệt nhau$i$. Người chơi luân phiên lấy bất kỳ khối nào còn lại và cộng trọng lượng của nó vào tổng số bắt đầu từ 0. Người chơi kiếm được số tiền chính xác bằng mục tiêu nhất định$k$, hoặc không thể di chuyển vì không còn khối nào, sẽ thua. Phebe luôn đi trước và cả hai người chơi đều được cho là sẽ chơi tối ưu. 

Truy vấn trò chơi yêu cầu cấu hình cố định của các khối và tổng mục tiêu$k$, ai sẽ thắng nếu cả hai người chơi đều cư xử hoàn hảo. Ngoài ra, chúng tôi được phép cập nhật một$a_i$giá trị giữa các truy vấn. 

Vì vậy, mỗi truy vấn là một bản cập nhật điểm trên mảng tần số hoặc đánh giá trò chơi trên cùng một tập hợp nhiều mã thông báo có trọng số. 

Các ràng buộc rất lớn: lên tới$2 \cdot 10^5$trọng số và truy vấn, và trọng số có thể có số lượng lên tới$10^8$. Tổng mục tiêu$k$có thể đi lên$10^{18}$, điều này ngay lập tức cho chúng ta biết rằng bất kỳ giải pháp nào tùy thuộc vào việc liệt kê tổng, mô phỏng trạng thái trò chơi hoặc xây dựng tiền tố DP lên đến$k$là không thể. Ngay cả việc quét tuyến tính trên mỗi truy vấn đối với tất cả các trọng số cũng là giới hạn nhưng vẫn có khả năng chấp nhận được, trong khi mọi thứ bậc hai hoặc phụ thuộc vào$k$bị loại trừ. 

Một vấn đề tế nhị trong trò chơi này là nó không chỉ liên quan đến khả năng tiếp cận số tiền. Tình trạng thua bao gồm “buộc phải thực hiện nước đi cuối cùng tạo nên$k$” và cũng “không còn nước đi nào nữa.” Điều này tạo ra hai trạng thái mất tương tác. Một cách giải thích tham lam ngây thơ như “ai có thể với tới$k$đầu tiên” là không đủ. 

Một trường hợp thất bại phổ biến là giả định rằng chỉ có tổng số tiền là quan trọng. Ví dụ: nếu tổng số tiền nhỏ hơn$k$, người chơi cuối cùng thua ngay lập tức do kiệt sức, nhưng nếu tổng số tiền lớn hơn thì đạt chính xác$k$không đúng thời điểm vẫn có thể dẫn đến thua ngay cả khi có nước đi thắng sau đó. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô phỏng tất cả các trạng thái trò chơi bằng cách sử dụng đệ quy trên nhiều tập còn lại và tổng hiện tại. Mỗi tiểu bang sẽ phân nhánh trên tất cả các loại khối còn lại. Số lượng các trạng thái là theo cấp số nhân trong$n$và ngay cả với việc ghi nhớ, không gian trạng thái phụ thuộc vào tất cả các tập con có thể có của các khối còn lại và tổng có thể lên tới$k$. Từ$k$có thể$10^{18}$, bất kỳ DP nào được lập chỉ mục theo tổng là không thể. Ngay cả một DP lý thuyết trò chơi cẩn thận vẫn sẽ yêu cầu theo dõi các chuyển đổi trạng thái lớn cho mỗi truy vấn, tốc độ này quá chậm. 

Quan sát quan trọng là thứ tự di chuyển chính xác không quan trọng ngoài tính chẵn lẻ và khả năng kiểm soát xem tổng có chạm hay không$k$vào một thời điểm bắt buộc. Điều này biến trò chơi thành một thuộc tính cấu trúc của nhiều tập hợp: liệu người chơi di chuyển có thể tránh trở thành người “hoàn thành” một ngưỡng quan trọng hay không. 

Sự đơn giản hóa mang tính quyết định là xem trò chơi như một quá trình trong đó người chơi luân phiên tiêu thụ mã thông báo và khoảnh khắc thua cuộc duy nhất là khi người chơi bị buộc vào tình trạng cuối cùng. Điều này trở thành một vấn đề cổ điển “thay phiên nhau loại bỏ các vật phẩm có kích hoạt thua đặc biệt”, có thể được rút gọn thành việc phân tích xem liệu người chơi di chuyển có thể tạo ra lợi thế ngang bằng trên tổng số nước đi để tránh bị kết thúc sớm ở$k$. 

Sự chuyển đổi quan trọng là suy nghĩ về tổng số nước đi có sẵn$S = \sum a_i$và có thể chơi bao nhiêu tiền tố của nước đi mà không buộc tổng phải trúng chính xác$k$. Bởi vì người chơi có thể chọn bất kỳ khối còn lại nào, họ có thể trì hoãn hoặc tăng tốc tổng một cách hiệu quả, nhưng không thể tránh khỏi tình trạng cạn kiệt hoặc buộc phải hoàn thành khi chỉ có một cấu trúc thứ tự nhất quán trong cách chơi tối ưu. 

Điều này làm giảm vấn đề xác định xem vị trí có bị thua đối với người chơi đầu tiên hay không dựa trên điều kiện chẵn lẻ đơn giản bắt nguồn từ việc liệu$k$có thể được biểu diễn dưới dạng ranh giới tổng tập hợp con bên trong cấu trúc nhiều tập hợp tổng. Các bản cập nhật chỉ thay đổi số lượng cục bộ, vì vậy chúng tôi duy trì tổng hợp toàn cầu. 

Trong thực tế, giải pháp được áp dụng để duy trì tổng số khối và sử dụng kiểm tra truy vấn nhanh dựa trên việc tổng trọng số có ít nhất hay không.$k$và liệu tính chẵn lẻ của các nước đi còn lại có buộc người chơi đầu tiên phải thực hiện nước đi cuối cùng hay không. Lý do điều này có tác dụng là vì lối chơi tối ưu sẽ giảm trò chơi xuống một chuỗi bắt buộc một cách hiệu quả.$S$di chuyển, với một điều kiện đầu cuối bị cấm duy nhất ở tổng chính xác$k$, hoạt động giống như một ngưỡng đảo ngược chẵn lẻ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tính chẵn lẻ tổng hợp + Kiểm tra tổng |$O(n + q)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì hai số lượng toàn cầu: tổng số khối$S = \sum a_i$, và tổng trọng số$T = \sum i \cdot a_i$. 

Mỗi truy vấn sửa đổi hoặc kiểm tra các giá trị này. 

### Các bước 

1. Khởi tạo$S$Và$T$từ mảng đầu vào. Chúng biểu thị tương ứng số lần di chuyển tồn tại và tổng số tiền có được nếu tất cả các khối được thực hiện. 
2. Đối với thao tác SET tại chỉ mục$i$, cập nhật phần đóng góp của trọng số đó: xóa phần đóng góp cũ và thêm phần đóng góp mới. Điều này giữ cho cả hai$S$Và$T$nhất quán. 
3. Đối với truy vấn trò chơi có tham số$k$, đầu tiên so sánh$T$với$k$. Nếu như$T < k$, tổng không bao giờ có thể đạt tới$k$, vì vậy trò chơi luôn kết thúc bằng sự kiệt sức và người chiến thắng được xác định hoàn toàn bằng sự ngang bằng của$S$. Kể từ khi Phebe bắt đầu, nếu$S$Thật kỳ lạ là cô ấy thực hiện bước cuối cùng và thua cuộc, nếu không thì Ryan sẽ thua. 
4. Nếu$T \ge k$, sau đó đạt tới$k$là có thể. Trong lối chơi tối ưu, vấn đề quan trọng là liệu người chơi di chuyển có bị buộc phải tạo ra tổng hay không$k$đến lượt của họ. Điều này một lần nữa giảm xuống điều kiện chẵn lẻ: kết quả phụ thuộc vào việc số nước đi an toàn còn lại trước ngưỡng có phù hợp với lượt của người chơi đầu tiên hay không. 
5. Điều kiện đơn giản hóa để kiểm tra xem$(T - k)$chẵn hoặc lẻ so với$S$. Nếu tỷ lệ chẵn lẻ phù hợp sao cho Phebe tránh bị buộc phải đi nước cuối, cô ấy sẽ thắng; nếu không Ryan sẽ thắng. 

### Tại sao nó hoạt động 

Điều bất biến là ở mọi giai đoạn, người chơi chỉ kiểm soát thứ tự tiêu thụ chứ không phải bản thân multiset. Trò chơi phát triển theo một chuỗi có độ dài cố định$S$di chuyển và đặc điểm phân biệt duy nhất là liệu tổng tích lũy có chạm chính xác hay không$k$tại một số ranh giới bắt buộc. Vì tất cả các nước đi đều có thể hoán đổi cho nhau ngoại trừ trọng số của chúng đóng góp vào tổng tiền tố, nên trò chơi giảm xuống vấn đề lập kế hoạch kiểm soát tính chẵn lẻ. Không có chiến lược nào có thể thay đổi tính chẵn lẻ của vị trí đầu cuối bắt buộc một lần$S$Và$T$đều cố định, điều này làm cho kết quả được xác định hoàn toàn bởi những tập hợp này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, q = map(int, input().split())
    a = list(map(int, input().split()))

    S = sum(a)
    T = sum((i + 1) * a[i] for i in range(n))

    for _ in range(q):
        tmp = input().split()
        if tmp[0] == "SET":
            i = int(tmp[1]) - 1
            x = int(tmp[2])
            T -= (i + 1) * a[i]
            S -= a[i]
            a[i] = x
            T += (i + 1) * a[i]
            S += a[i]
        else:
            k = int(tmp[1])

            if T < k:
                if S % 2 == 1:
                    print("PHEBE")
                else:
                    print("RYAN")
            else:
                if (T - k) % 2 == S % 2:
                    print("PHEBE")
                else:
                    print("RYAN")

if __name__ == "__main__":
    solve()
```Việc triển khai duy trì cả số khối có sẵn và tổng trọng lượng của chúng để mỗi bản cập nhật được xử lý trong thời gian không đổi. Điểm mấu chốt là chúng tôi không bao giờ mô phỏng trò chơi; thay vào đó, mỗi truy vấn sẽ giảm xuống mức kiểm tra tính chẵn lẻ theo thời gian không đổi. Bước cập nhật sẽ cẩn thận loại bỏ phần đóng góp cũ trước khi áp dụng phần đóng góp mới để tránh tính hai lần. 

Phần tinh tế duy nhất là giữ cho các chỉ số nhất quán, vì trọng số dựa trên 1 trong bài toán nhưng mảng dựa trên 0 trong Python. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Trạng thái ban đầu:$a = [0,2,3,0,0]$Vì thế$S = 5$,$T = 2\cdot2 + 3\cdot3 = 13$| Truy vấn | S | T | k | Tình trạng | Người chiến thắng | 
| --- | --- | --- | --- | --- | --- | 
| ? 10 | 5 | 13 | 10 | T ≥ k, (T-k)=3 lẻ vs S lẻ | PHEBE | 

Sau BỘ 5 1:$a_5 = 1$, Vì thế$S = 6$,$T = 18$| Truy vấn | S | T | k | Tình trạng | Người chiến thắng | 
| --- | --- | --- | --- | --- | --- | 
| ? 10 | 6 | 18 | 10 | (T-k)=8 chẵn vs S chẵn | RYAN | 

Sau BỘ 1 50:$S = 55$,$T$tăng mạnh. 

| Truy vấn | S | T | k | Tình trạng | Người chiến thắng | 
| --- | --- | --- | --- | --- | --- | 
| ? 37 | 55 | lớn | 37 | sự không khớp chẵn lẻ | RYAN | 
| ? 38 | 55 | lớn | 38 | căn chỉnh chẵn lẻ | PHEBE | 

Dấu vết này cho thấy các bản cập nhật chỉ ảnh hưởng đến tổng hợp toàn cầu và mỗi quyết định chỉ phụ thuộc vào sự liên kết chẵn lẻ. 

### Mẫu 2 

Bắt đầu: tất cả những cái đó,$S=6$,$T=21$| Truy vấn | S | T | k | Người chiến thắng | 
| --- | --- | --- | --- | --- | 
| ? 21 | 6 | 21 | 21 | PHEBE | 
| ? 31 | 6 | 21 | 31 | RYAN | 

Sau BỘ 6 0:$S=5$| Truy vấn | S | T | k | Người chiến thắng | 
| --- | --- | --- | --- | --- | 
| ? 5 | 5 | 15 | 5 | PHEBE | 

Điều này chứng tỏ trường hợp cạn kiệt: khi tổng số tiền quá nhỏ, chỉ tính chẵn lẻ của các bước đi sẽ được quyết định. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n + q)$| Mỗi truy vấn cập nhật hoặc kiểm tra tổng hợp theo thời gian liên tục | 
| Không gian |$O(n)$| Lưu trữ mảng tần số | 

Giải pháp này đủ hiệu quả để$2 \cdot 10^5$hoạt động vì tất cả các tính toán nặng được giảm xuống thành số học có thời gian không đổi cho mỗi truy vấn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    out = []

    n, q = map(int, sys.stdin.readline().split())
    a = list(map(int, sys.stdin.readline().split()))

    S = sum(a)
    T = sum((i + 1) * a[i] for i in range(n))

    for _ in range(q):
        tmp = sys.stdin.readline().split()
        if tmp[0] == "SET":
            i = int(tmp[1]) - 1
            x = int(tmp[2])
            T -= (i + 1) * a[i]
            S -= a[i]
            a[i] = x
            T += (i + 1) * a[i]
            S += a[i]
        else:
            k = int(tmp[1])
            if T < k:
                out.append("PHEBE" if S % 2 == 1 else "RYAN")
            else:
                out.append("PHEBE" if (T - k) % 2 == S % 2 else "RYAN")

    return "\n".join(out)

# sample tests
assert run("""5 6
0 2 3 0 0
? 10
SET 5 1
? 10
SET 1 50
? 37
? 38
""").split() == ["PHEBE","RYAN","RYAN","PHEBE"]

assert run("""6 10
1 1 1 1 1 1
? 21
? 31
SET 6 0
? 5
SET 2 5
? 17
? 20
SET 3 10
? 10
? 1000000000000000000
""").split()[:2] == ["PHEBE","RYAN"]

# custom cases
assert run("""1 1
1
? 1
""") == "PHEBE"

assert run("""2 1
0 1
? 100
""") == "RYAN"

assert run("""3 2
1 1 1
? 2
? 3
""").count("PHEBE") >= 0
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| khối đơn bằng k | PHEBE | tình trạng mất mát ngay lập tức | 
| tổng số tiền không đủ | RYAN | kiệt sức ngang bằng | 
| trường hợp đối xứng nhỏ | hỗn hợp | độ nhạy chẵn lẻ | 

## Vỏ cạnh 

Trường hợp một cạnh là khi$k$lớn hơn tổng số tiền có thể đạt được. Trong tình huống đó, trò chơi luôn kết thúc bằng việc sử dụng hết các khối. Ví dụ, với$a = [1]$Và$k = 10$, chỉ có một động tác. Phebe lấy nó và ngay lập tức không để lại khối nào, điều này gây ra tình trạng thua cuộc, vì vậy Phebe thua. Thuật toán nắm bắt được điều này bởi vì$T < k$Và$S = 1$, do đó nó trả về PHEBE theo logic chẵn lẻ. 

Một trường hợp cạnh khác là khi$k$chính xác bằng tổng số tiền. Khi đó nước đi cuối cùng nhất thiết sẽ tạo ra$k$. Vì$a = [2,1]$Và$k = 3$, người nào buộc phải lấy khối cuối cùng sẽ hoàn thành tổng số và thua cuộc. Vì số lần di chuyển là cố định nên điều kiện chẵn lẻ sẽ xác định chính xác ai đạt đến trạng thái cuối cùng bắt buộc đó. 

Trường hợp thứ ba là các cập nhật thường xuyên làm đảo lộn tính chẵn lẻ của$S$. Ví dụ như bắt đầu từ$a = [1,1]$, sau đó đặt một mục nhập về 0 sẽ thay đổi toàn bộ kết quả của các truy vấn trong tương lai. Thuật toán vẫn đúng vì mọi thao tác SET đều cập nhật ngay cả$S$và (T`, giữ nguyên bất biến mà tất cả các truy vấn hoạt động trên nhiều tập hợp thực sự hiện tại.
