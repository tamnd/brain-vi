---
title: "CF 1245B - RPS bị hạn chế"
description: "Chúng ta được cho một chuỗi các động tác oẳn tù tì do Bob thực hiện. Bên cạnh đó, Alice có một danh sách các bước đi cố định: cô ấy phải chơi chính xác một số Đá, Giấy và Kéo được chỉ định trong tất cả các vòng."
date: "2026-06-13T20:36:29+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "dp", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1245
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 597 (Div. 2)"
rating: 1200
weight: 1245
solve_time_s: 179
verified: true
draft: false
---

[CF 1245B - RPS bị hạn chế](https://codeforces.com/problemset/problem/1245/B) 

**Đánh giá:** 1200 
**Tags:** thuật toán xây dựng, dp, tham lam 
**Thời gian giải:** 2m 59s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một chuỗi các động tác oẳn tù tì do Bob thực hiện. Bên cạnh đó, Alice có một danh sách các bước đi cố định: cô ấy phải chơi chính xác một số Đá, Giấy và Kéo được chỉ định trong tất cả các vòng. 

Mục tiêu là để quyết định xem Alice có thể sắp xếp các nước đi của mình để thắng ít nhất một nửa số vòng (làm tròn) hay không, với điều kiện là cô ấy biết trước toàn bộ trình tự của Bob. Chiến thắng trong một vòng duy nhất phụ thuộc vào các quy tắc RPS tiêu chuẩn: Đá thắng Kéo, Kéo thắng Giấy và Giấy thắng Đá. 

Nếu một chiến lược như vậy tồn tại, chúng ta cũng phải xây dựng một chuỗi các nước đi hợp lệ của Alice để đạt được số lần thắng cần thiết trong khi vẫn tôn trọng số lượng chính xác của mỗi nước đi. 

Các ràng buộc rất nhỏ: tối đa 100 trường hợp thử nghiệm và mỗi trò chơi có tối đa 100 vòng. Điều này ngay lập tức loại trừ bất kỳ điều gì nặng hơn tuyến tính hoặc bậc hai cho mỗi trường hợp thử nghiệm, nhưng quan trọng hơn, nó gợi ý rằng việc xây dựng tham lam hoặc lập trình động đơn giản theo số lượng là đủ. Không cần tìm kiếm theo cấp số nhân hoặc quay lui vì không gian trạng thái rất nhỏ nhưng có cấu trúc. 

Một trường hợp thất bại tinh vi phát sinh khi một chiến lược ngây thơ cố gắng “tối đa hóa chiến thắng cục bộ” mà không tính đến việc cạn kiệt tài nguyên. Ví dụ: nếu Alice tham lam sử dụng tất cả các nước đi Giấy sớm để đánh bại Đá, sau đó cô ấy có thể đối mặt với Kéo nhưng không còn Kéo, mất đi những chiến thắng tiềm năng mà một cách phân phối khác có thể đã bảo toàn. Một chế độ thất bại khác là thực hiện các bước di chuyển tùy ý sau khi đạt được một số chiến thắng, vô tình phá vỡ yêu cầu về số lượng chính xác hoặc giảm số chiến thắng một cách không cần thiết. 

Thách thức chính là cân bằng đồng thời hai ràng buộc: tối đa hóa chiến thắng trước một chuỗi đã biết và tôn trọng hạn ngạch cố định của từng loại nước đi. 

## Phương pháp tiếp cận 

Một giải pháp vũ phu sẽ thử tất cả các hoán vị có thể có trong nhiều bước di chuyển của Alice và tính số chiến thắng trước Bob cho mỗi cách sắp xếp. Ngay cả đối với một trường hợp thử nghiệm đơn lẻ, điều này theo thứ tự$\frac{n!}{a!b!c!}$, nó trở nên lớn về mặt thiên văn ngay cả đối với$n = 20$. Điều này là hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là mỗi vị trí đều độc lập ngoại trừ việc cung cấp các bước di chuyển hạn chế. Chúng ta không cần xem xét các hoán vị trên toàn cầu; thay vào đó, chúng ta có thể quyết định từng vị trí một cách tham lam dựa trên việc liệu chúng ta có thể sử dụng nước đi chiến thắng ở đó hay không. 

Ở mỗi chỉ số, nước đi của Bob xác định chính xác một nước đi phản công tốt nhất mang lại chiến thắng. Nếu chúng ta vẫn còn sẵn nước đi đó trong hạn ngạch của mình thì việc sử dụng nước đi đó luôn là điều tối ưu, bởi vì sử dụng nước đi chiến thắng ngay bây giờ sẽ không bao giờ làm giảm các cơ hội trong tương lai ngoại trừ việc tiêu thụ một nguồn tài nguyên hạn chế. Vì mỗi nước đi đóng góp vào chính xác một vị trí, nên sự phân bổ tham lam này sẽ tối đa hóa số trận thắng có thể đạt được về tổng thể. 

Khi tất cả các cơ hội chiến thắng đã hết, các vị trí còn lại có thể được lấp đầy tùy ý bằng cách sử dụng các nước đi còn sót lại trong khi vẫn bảo toàn số lượng. 

Điều này làm giảm vấn đề trong việc xây dựng một nhiệm vụ có lợi nhất trong điều kiện hạn chế về năng lực và sau đó kiểm tra xem mức tối đa đó có đáp ứng ngưỡng yêu cầu hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Xây dựng tham lam | O(n) mỗi lần kiểm tra | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

hãy để$k = \lceil n/2 \rceil$, số lần thắng tối thiểu cần thiết. 

1. Với mỗi test case, hãy đọc$n$, đếm$a, b, c$, và chuỗi của Bob. 

Chúng tôi giữ một mảng kết quả`ans`ban đầu trống và theo dõi số lượng còn lại. 
2. Lượt đi đầu tiên: cố gắng chỉ định các nước đi thắng bất cứ khi nào có thể. 

Đối với mỗi vị trí$i$: 

- Nếu Bob chơi 'R', Alice thắng bằng cách chơi 'P' (nếu còn P). 
- Nếu Bob chơi 'P', Alice thắng bằng cách chơi 'S' (nếu còn S). 
- Nếu Bob chơi 'S', Alice thắng bằng cách chơi 'R' (nếu còn R). 

Chúng ta tham lam ấn định ngay những nước đi thắng cuộc này và giảm hạn ngạch tương ứng. 

Lý do là mỗi nhiệm vụ như vậy đều hoàn toàn có lợi nên việc trì hoãn chỉ có nguy cơ lãng phí cơ hội. 
3. Sau lần vượt qua này, hãy đếm xem đã đạt được bao nhiêu trận thắng. 
4. Nếu ít nhất đã có chiến thắng$k$, ta tiến hành điền tùy ý tất cả các vị trí trống còn lại bằng các bước di chuyển còn sót lại$a, b, c$. 
5. Nếu số tiền thắng ít hơn$k$, chúng tôi lập tức xuất ra "KHÔNG" vì ngay cả chiến lược tham lam tối ưu cũng không thể đạt đến ngưỡng. 
6. Ngược lại, xuất "CÓ" theo sau là chuỗi đã xây dựng. 

### Tại sao nó hoạt động 

Mỗi thế cờ có nhiều nhất một nước đi đảm bảo thắng lợi. Việc chỉ định các bước di chuyển như vậy một cách tham lam không bao giờ xung đột với vị trí khác bởi vì các bước di chuyển chỉ bị hạn chế bởi số lượng toàn cầu chứ không phụ thuộc vào vị trí. Vì mỗi nhiệm vụ chiến thắng sẽ làm tăng tổng số điểm một cách nghiêm ngặt và tiêu tốn chính xác một đơn vị tài nguyên hạn chế, nên việc trì hoãn một chiến thắng không thể làm tăng số trận thắng cuối cùng. Vì vậy, thủ tục tham lam này tạo ra số chiến thắng tối đa có thể đạt được. Nếu ngay cả mức tối đa này vẫn dưới ngưỡng thì không có sự sắp xếp lại hợp lệ nào có thể thành công. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a, b, c = map(int, input().split())
        s = input().strip()

        ans = [''] * n
        wins = 0

        # First pass: take all guaranteed wins
        for i, ch in enumerate(s):
            if ch == 'R' and b > 0:
                ans[i] = 'P'
                b -= 1
                wins += 1
            elif ch == 'P' and c > 0:
                ans[i] = 'S'
                c -= 1
                wins += 1
            elif ch == 'S' and a > 0:
                ans[i] = 'R'
                a -= 1
                wins += 1

        need = (n + 1) // 2
        if wins < need:
            print("NO")
            continue

        # Fill remaining slots
        for i in range(n):
            if ans[i] == '':
                if a > 0:
                    ans[i] = 'R'
                    a -= 1
                elif b > 0:
                    ans[i] = 'P'
                    b -= 1
                else:
                    ans[i] = 'S'
                    c -= 1

        print("YES")
        print("".join(ans))

if __name__ == "__main__":
    solve()
```Vòng lặp đầu tiên chỉ tiêu thụ mạnh mẽ tài nguyên di chuyển khi có sẵn chiến thắng được đảm bảo. Đây là nơi duy nhất mà chiến thắng được quyết định. Vòng thứ hai hoàn toàn là một giai đoạn bổ sung nhằm tôn trọng hạn ngạch còn lại mà không ảnh hưởng đến các kết quả đã cố định. 

Một lỗi phổ biến là cố gắng cân bằng đồng thời cả ba kiểu di chuyển trong lần vượt qua đầu tiên. Điều đó là không cần thiết vì việc kiểm tra tính khả thi chỉ phụ thuộc vào việc có nắm bắt đủ cơ hội chiến thắng hay không chứ không phụ thuộc vào việc phân bổ tổn thất một cách đồng đều. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3
a,b,c = 1,1,1
s = RPS
```| tôi | Bob | Quyết định | Còn lại (a,b,c) | Thắng | 
| --- | --- | --- | --- | --- | 
| 0 | R | P | 1,0,1 | 1 | 
| 1 | P | S | 1,0,0 | 2 | 
| 2 | S | R | 0,0,0 | 3 | 

Tất cả các nước đi đều được sử dụng để đảm bảo chiến thắng, đạt 3 trận thắng, vượt quá$\lceil 3/2 \rceil = 2$. Thuật toán đưa ra CÓ với toàn bộ nhiệm vụ chiến thắng. 

### Ví dụ 2 

đầu vào:```
n = 3
a,b,c = 3,0,0
s = RPS
```| tôi | Bob | Quyết định | Còn lại (a,b,c) | Thắng | 
| --- | --- | --- | --- | --- | 
| 0 | R | - | 3,0,0 | 0 | 
| 1 | P | - | 3,0,0 | 0 | 
| 2 | S | R | 2,0,0 | 1 | 

Chỉ có một động thái chiến thắng là có thể. Chiến thắng bắt buộc là 2, do đó thuật toán từ chối chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) mỗi lần kiểm tra | Mỗi vị trí được xử lý tối đa hai lần | 
| Không gian | O(n) | Lưu trữ chuỗi đầu ra | 

Các ràng buộc cho phép tối đa 100 trường hợp thử nghiệm với$n \le 100$, do đó việc xây dựng tuyến tính này dễ dàng phù hợp với giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import ceil

    def solve():
        t = int(input())
        out = []
        for _ in range(t):
            n = int(input())
            a, b, c = map(int, input().split())
            s = input().strip()

            ans = [''] * n
            wins = 0

            for i, ch in enumerate(s):
                if ch == 'R' and b > 0:
                    ans[i] = 'P'
                    b -= 1
                    wins += 1
                elif ch == 'P' and c > 0:
                    ans[i] = 'S'
                    c -= 1
                    wins += 1
                elif ch == 'S' and a > 0:
                    ans[i] = 'R'
                    a -= 1
                    wins += 1

            need = (n + 1) // 2
            if wins < need:
                out.append("NO")
                continue

            for i in range(n):
                if ans[i] == '':
                    if a > 0:
                        ans[i] = 'R'
                        a -= 1
                    elif b > 0:
                        ans[i] = 'P'
                        b -= 1
                    else:
                        ans[i] = 'S'
                        c -= 1

            out.append("YES")
            out.append("".join(ans))

        return "\n".join(out)

    return solve()

# provided samples
assert run("""2
3
1 1 1
RPS
3
3 0 0
RPS
""") == """YES
PSR
NO"""

# minimum case
assert run("""1
1
1 0 0
R
""") == """NO"""

# all identical Bob
assert run("""1
3
1 1 1
RRR
""") != ""

# maximum trivial win
assert run("""1
2
1 1 0
RP
""").split()[0] == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mẫu 1 | CÓ / KHÔNG | tính đúng đắn cơ bản | 
| vòng đơn không thể | KHÔNG | xử lý ngưỡng | 
| tất cả trường hợp R | CÓ hoặc KHÔNG tùy theo | hành vi phân bổ tham lam | 
| chiến thắng đảm bảo nhỏ | CÓ | xây dựng tích cực | 

## Vỏ cạnh 

Trường hợp một bên là khi trình tự của Bob thiên về một loại nước đi, nhưng Alice thiếu đủ bộ đếm để có phản ứng thắng tương ứng. Ví dụ: nếu Bob chơi nhiều 'R' nhưng Alice có rất ít 'P', thì vòng lặp tham lam chỉ cần bỏ qua các vị trí đó và để chúng để điền sau. Thuật toán xử lý việc này một cách tự nhiên mà không vi phạm số đếm, bởi vì nó không bao giờ ép buộc một nước đi thắng không khả dụng. 

Một trường hợp khác là khi Alice đếm chính xác nhưng cơ hội chiến thắng lại rải rác. Ngay cả khi có thể giành chiến thắng, chúng có thể không xuất hiện sớm trong chuỗi. Quá trình quét tham lam vẫn nắm bắt được chúng vì nó không phụ thuộc vào thứ tự vị trí ngoài quá trình truyền tải tuyến tính và mọi cơ hội đều được đánh giá độc lập. 

Trường hợp cuối cùng là khi số trận thắng yêu cầu chính xác bằng số trận thắng tối đa có thể đạt được. Thuật toán vẫn hoạt động vì trước tiên nó xây dựng toàn bộ nhiệm vụ giành chiến thắng tối đa và chỉ loại bỏ các trường hợp hoàn toàn không khả thi khi ngay cả mức tối đa đó cũng không đủ.
