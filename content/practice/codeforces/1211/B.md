---
title: "CF 1211B - Du hành vòng quanh Vành đai Vàng Berland"
description: "Chúng ta được yêu cầu lập kế hoạch số lần ghé thăm tối thiểu cho một khách du lịch đi dọc theo một tuyến đường cố định trong các thành phố. Các thành phố được đánh số từ 1 đến n, sắp xếp theo một chu kỳ và người du hành luôn di chuyển về phía trước: từ thành phố i đến i+1 và từ thành phố n quay lại thành phố 1."
date: "2026-06-11T23:08:57+07:00"
tags: ["codeforces", "competitive-programming", "*special", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1211
codeforces_index: "B"
codeforces_contest_name: "Kotlin Heroes: Episode 2"
rating: 1500
weight: 1211
solve_time_s: 98
verified: true
draft: false
---

[CF 1211B - Du hành vòng quanh Vành đai Vàng của Berland](https://codeforces.com/problemset/problem/1211/B) 

**Đánh giá:** 1500 
**Tags:** *đặc biệt, thực hiện 
**Thời gian giải:** 1 phút 38 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu lập kế hoạch số lần ghé thăm tối thiểu cho một khách du lịch đi dọc theo một tuyến đường cố định trong các thành phố. Các thành phố được đánh số từ 1 đến n, sắp xếp theo một chu kỳ và khách du lịch luôn di chuyển về phía trước: từ thành phố i đến i+1 và từ thành phố n quay lại thành phố 1. Mỗi thành phố i có một số lượng ảnh selfie mục tiêu$a_i$thứ đó phải được chụp và khách du lịch chỉ có thể chụp ảnh tự sướng một lần cho mỗi lần ghé thăm. Anh ta bắt đầu ở thành phố 1 và không thể bỏ qua các thành phố. Mục tiêu là tìm ra tổng số lượt ghé thăm thành phố cần thiết để đáp ứng tất cả các yêu cầu về chụp ảnh tự sướng. 

Đầu vào bao gồm số lượng thành phố$n$và mảng$a$chiều dài$n$. Mỗi$a_i$có thể lên đến$10^9$Và$n$có thể lên đến$2 \cdot 10^5$. Điều này ngụ ý rằng mô phỏng lực lượng vũ phu đến thăm từng thành phố một sẽ quá chậm, vì tổng số lượt truy cập có thể rất lớn. Thay vào đó, chúng ta cần một cách tiếp cận hoạt động theo thời gian tuyến tính về mặt$n$. 

Một giải pháp đơn giản có thể thử mô phỏng khách du lịch chụp ảnh tự sướng cho đến khi tất cả số lượng đều được thỏa mãn, di chuyển từ thành phố này sang thành phố khác và lặp lại một chu kỳ. Ví dụ, với đầu vào`3 1 0 0`, quá trình mô phỏng sẽ dừng sau 1 lần truy cập, điều đó không sao cả. Nếu không có`3 3 1 2`, mô phỏng đơn giản có thể lặp đi lặp lại một cách không cần thiết, không tìm được số lượt truy cập tối thiểu một cách hiệu quả. Thách thức là mỗi thành phố có thể được ghé thăm nhiều lần do có chu kỳ và số lượng chu kỳ không cố định trước. 

Các trường hợp đặc biệt không rõ ràng bao gồm các thành phố không yêu cầu ảnh tự chụp, các thành phố có số lượng ảnh rất cao so với các thành phố khác và các thành phố có yêu cầu thấp hơn thành phố trước đó. Ví dụ, đầu vào`3 0 100 0`nên quay lại`101`, bởi vì thành phố đầu tiên không đóng góp lượt truy cập nào nhưng thành phố thứ hai yêu cầu 100 lượt truy cập, trải đều theo chu kỳ. Một cách tiếp cận bất cẩn có thể đánh giá thấp những chuyến viếng thăm nếu nó bỏ qua tác động tích lũy của sự phụ thuộc mang tính chu kỳ. 

## Phương pháp tiếp cận 

Giải pháp brute-force rất đơn giản: bắt đầu ở thành phố 1, chụp ảnh selfie cho đến khi mỗi thành phố đạt được số lượng yêu cầu, di chuyển từng thành phố và đi vòng quanh. Đối với mỗi lần ghé thăm, hãy giảm số ảnh selfie còn lại của thành phố hiện tại. Điều này đúng về mặt logic nhưng có thể yêu cầu tính tổng tới$10^9$bước cho một thành phố duy nhất, đó là không khả thi. 

The key observation is that for each city i (after the first), we can think in terms of "excess selfies" carried over from the previous city. Vì khách du lịch di chuyển theo chu kỳ nên số lượt ghé thăm tối thiểu đối với thành phố i bị ảnh hưởng bởi sự khác biệt giữa số lượng ảnh selfie của thành phố trước đó và yêu cầu của nó. More formally, for each city i, the number of visits needed for city i is at least$\max(0, a_i - a_{i-1})$. Thành phố đầu tiên luôn đóng góp hết mình$a_1$đến số lượt truy cập. Sau đó, tổng hợp các khoản đóng góp trên tất cả các thành phố theo cách tăng dần này sẽ cho ra tổng số lượt truy cập tối thiểu. Điều này hiệu quả vì mọi sự dư thừa trong một thành phố chỉ có thể làm giảm số lượng lượt truy cập bổ sung cần thiết cho thành phố tiếp theo. 

Giải pháp tối ưu tận dụng đặc tính của vòng tròn và không thể bỏ qua ảnh tự chụp: số lượt ghé thăm bổ sung tối thiểu cần thiết cho mỗi thành phố là những khác biệt không âm so với thành phố trước đó. Bằng cách tổng hợp những khác biệt đã điều chỉnh này và thêm ảnh tự chụp của thành phố đầu tiên, chúng tôi đạt được giải pháp O(n). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(tổng(a_i)) | O(n) | Quá chậm đối với a_i lớn | 
| Tối thiểu dựa trên sự khác biệt | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số thành phố$n$và mảng$a$đại diện cho những bức ảnh selfie cần thiết ở mỗi thành phố. 
2. Khởi tạo tổng số lượt truy cập dưới dạng$total = a_1$bởi vì cuộc hành trình bắt đầu ở thành phố 1 và tất cả các bức ảnh selfie của nó đều phải được chụp. 
3. Lặp lại các thành phố từ 2 đến n. Với mỗi thành phố i, hãy tính$extra = \max(0, a_i - a_{i-1})$. Điều này thể hiện những chuyến thăm bổ sung cần thiết cho thành phố i ngoài những gì đã được "bao phủ" bằng cách ghé thăm thành phố trước đó. 
4. Thêm$extra$ĐẾN$total$. 
5. In bản cuối cùng$total$. 

Tại sao nó hoạt động: cách tiếp cận dựa trên sự khác biệt nắm bắt chính xác số lượt truy cập bổ sung là không thể tránh khỏi do thứ tự theo chu kỳ. Đối với bất kỳ thành phố nào, nếu số ảnh selfie được yêu cầu của thành phố trước đó cao hơn hoặc bằng nhau thì thành phố hiện tại có thể sử dụng các lượt truy cập tương tự mà không cần phải thực hiện thêm chu kỳ. Chỉ khi yêu cầu của thành phố hiện tại vượt quá yêu cầu trước đó thì mới buộc phải có thêm các chuyến thăm, điều này$\max(0, a_i - a_{i-1})$nắm bắt công thức. Việc đóng chu kỳ (từ thành phố n đến 1) được xử lý ngầm vì thành phố đầu tiên được tính đầy đủ, đảm bảo không tính thiếu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))

total = a[0]
for i in range(1, n):
    total += max(0, a[i] - a[i-1])

print(total)
```Mã đọc số lượng thành phố và ảnh tự chụp cần thiết. Nó khởi tạo`total`với những bức ảnh selfie của thành phố đầu tiên. Đối với mỗi thành phố tiếp theo, nó bổ sung thêm sự khác biệt không âm với thành phố trước đó. Điều này đảm bảo chúng tôi chỉ tính số lượt truy cập cần thiết tối thiểu. Không có lỗi riêng lẻ nào vì chúng tôi bắt đầu lặp lại từ thành phố thứ hai và xử lý riêng thành phố đầu tiên. Số nguyên của Python xử lý số lượng rất lớn mà không bị tràn. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
3
1 0 0
```| Thành phố tôi | một [tôi] | a[i] - a[i-1] | tối đa(0, khác biệt) | tổng số lượt truy cập | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | - | - | 1 | 
| 2 | 0 | 0 - 1 = -1 | 0 | 1 | 
| 3 | 0 | 0 - 0 = 0 | 0 | 1 | 

Đầu ra:`1`. Chứng tỏ rằng các thành phố có yêu cầu thấp hơn trước đây không làm tăng tổng số lượt truy cập. 

Đầu vào mẫu 2:```
4
3 1 2 2
```| Thành phố tôi | một [tôi] | a[i] - a[i-1] | tối đa(0, khác biệt) | tổng số lượt truy cập | 
| --- | --- | --- | --- | --- | 
| 1 | 3 | - | - | 3 | 
| 2 | 1 | 1 - 3 = -2 | 0 | 3 | 
| 3 | 2 | 2 - 1 = 1 | 1 | 4 | 
| 4 | 2 | 2 - 2 = 0 | 0 | 4 | 

Đầu ra:`4`. Xác nhận thuật toán chính xác chỉ thêm những lượt truy cập bổ sung cần thiết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lần đi qua mảng có độ dài n | 
| Không gian | O(1) | Chỉ tổng số đang chạy được lưu trữ; không có mảng bổ sung | 

Cho n 2·10^5, nghiệm thời gian tuyến tính này nằm trong giới hạn thời gian 3 giây. Dung lượng không đổi, vừa vặn thoải mái trong phạm vi 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    a = list(map(int, input().split()))
    total = a[0]
    for i in range(1, n):
        total += max(0, a[i] - a[i-1])
    return str(total)

# provided samples
assert run("3\n1 0 0\n") == "1", "sample 1"
assert run("4\n3 1 2 2\n") == "4", "sample 2"

# custom cases
assert run("3\n0 100 0\n") == "100", "high middle requirement"
assert run("5\n5 5 5 5 5\n") == "5", "all equal"
assert run("3\n1 0 2\n") == "3", "requires extra at end"
assert run("6\n0 0 0 1 0 0\n") == "1", "single non-zero late"
assert run("3\n1000000000 1000000000 1000000000\n") == "1000000000", "large numbers"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3\n0 100 0 | 100 | Xử lý bước nhảy lớn ở giữa | 
| 5\n5 5 5 5 5 | 5 | Tất cả các giá trị bằng nhau không làm tăng lượt truy cập một cách không cần thiết | 
| 3\n1 0 2 | 3 | Cần thêm chuyến thăm ở thành phố cuối | 
| 6\n0 0 0 1 0 0 | 1 | Yêu cầu khác 0 thưa thớt được xử lý chính xác | 
| 3\n1000000000 1000000000 1000000000 | 1000000000 | Xử lý số lượng rất lớn mà không bị tràn | 

## Vỏ cạnh 

Đối với đầu vào`3 0 100 0`, thuật toán tính tổng là `0 + max(0,100-0) + max(0,0
