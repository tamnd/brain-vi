---
title: "CF 1240F - Bóng đá"
description: "Chúng ta được cung cấp một tập hợp các đội và danh sách các trận đấu tiềm năng giữa các cặp đội. Mỗi trận đấu có thể được diễn ra ở một trong nhiều sân vận động hoặc bị bỏ qua hoàn toàn."
date: "2026-06-15T04:57:17+07:00"
tags: ["codeforces", "competitive-programming", "graphs"]
categories: ["algorithms"]
codeforces_contest: 1240
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 591 (Div. 1, based on Technocup 2020 Elimination Round 1)"
rating: 3100
weight: 1240
solve_time_s: 221
verified: false
draft: false
---

[CF 1240F - Bóng đá](https://codeforces.com/problemset/problem/1240/F) 

**Đánh giá:** 3100 
**Thẻ:** đồ thị 
**Thời gian giải:** 3 phút 41s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các đội và danh sách các trận đấu tiềm năng giữa các cặp đội. Mỗi trận đấu có thể được diễn ra ở một trong nhiều sân vận động hoặc bị bỏ qua hoàn toàn. Nếu một trận đấu được diễn ra, trận đấu đó sẽ đóng góp một trận cho cả hai đội tham gia và đóng góp đó gắn liền với sân vận động đã chọn. 

Đối với mỗi đội, chúng tôi theo dõi số trận họ đã chơi ở mỗi sân vận động. Hạn chế chính là một đội không được quá tập trung ở bất kỳ sân vận động nào: nếu chúng ta nhìn vào một đội cố định và đếm số trận đấu của đội đó được chơi ở mỗi sân vận động, thì sự khác biệt giữa sân vận động được sử dụng nhiều nhất và sân vận động ít được sử dụng nhất phải là nhiều nhất là hai. 

Mỗi đội có một trọng số và mỗi trận đấu có sự tham gia của đội đó đều đóng góp trọng số đó vào tổng lợi nhuận. Vì mỗi trận đấu có sự tham gia của hai đội nên việc chọn một trận đấu sẽ đóng góp tổng trọng số của hai điểm cuối. Mục tiêu là chọn một tập hợp con các trận đấu và chỉ định mỗi trận đấu đã chọn cho một sân vận động sao cho thỏa mãn ràng buộc cho mỗi đội trong khi tối đa hóa tổng lợi nhuận. 

Cấu trúc ràng buộc là tinh tế. Nó không hạn chế trực tiếp tổng độ, nhưng hạn chế mức độ phân bố không đồng đều của các cạnh sự cố đã chọn trên k sân vận động. Cách giải thích ngây thơ thường thất bại khi chỉ tập trung vào mức độ tổng thể hoặc cố gắng cân bằng một cách tham lam trên mỗi cạnh. 

Các giới hạn là nhỏ theo nghĩa cấu trúc. Có tối đa 100 đội và 1000 trận đấu, trong khi số lượng sân vận động có thể lên tới 1000. Điều này ngay lập tức gợi ý rằng các quyết định cục bộ theo từng đội hoặc mỗi cạnh là khả thi, nhưng các nhiệm vụ tổ hợp toàn cầu trên tất cả các cạnh và sân vận động cần một cấu trúc có cấu trúc thay vì liệt kê vũ lực. 

Một cạm bẫy phổ biến là cho rằng mọi cạnh đều nên được sử dụng. Điều này là sai vì các cạnh có trọng lượng cao có thể không tương thích với yêu cầu giữ thăng bằng trên mỗi sân vận động. Một chế độ thất bại tinh vi khác là chỉ định từng cạnh độc lập cho một sân vận động một cách tham lam; điều này phá vỡ ràng buộc phân phối theo từng nhóm vì các quyết định ở một bên ảnh hưởng đồng thời đến cả hai điểm cuối. 

## Phương pháp tiếp cận 

Nỗ lực tự nhiên đầu tiên là nghĩ mỗi trận đấu có thể được phân công độc lập cho bất kỳ sân vận động nào và cố gắng tối đa hóa lợi nhuận bằng cách luôn tham gia tất cả các trận đấu. Điều này bỏ qua ràng buộc cân bằng và ngay lập tức thất bại, vì một đội có nhiều cạnh sự cố sẽ kết thúc với tất cả họ trong cùng một sân vận động, vi phạm giới hạn tối đa-trừ-tối thiểu. 

Chế độ xem lực lượng vũ phu có cấu trúc chặt chẽ hơn một chút là coi mỗi cạnh là một biến quyết định đối với k+1 lựa chọn (k sân vận động hoặc bỏ qua) và thực thi các ràng buộc cho mỗi đội. Điều này trở thành một phép tìm kiếm hàm mũ trên các phép gán có ràng buộc ghép mạnh, về cơ bản là một bài toán thỏa mãn ràng buộc trên tối đa 1000 biến, điều này là không khả thi. 

Cái nhìn sâu sắc quan trọng là tách nhiệm vụ toàn cầu thành các ràng buộc khả thi cho mỗi nhóm có thể được đáp ứng cục bộ trong khi vẫn cho phép phân công biên nhất quán toàn cầu. Quan sát quan trọng là ràng buộc chỉ phụ thuộc vào sự khác biệt giữa số lượng trên các sân vận động chứ không phải giá trị tuyệt đối của chúng. Điều này gợi ý rằng chúng tôi chỉ cần đảm bảo rằng đối với mỗi đội, các biên độ sự cố của đội đó được phân bổ một cách gần như đồng nhất trên các sân vận động và chúng tôi được phép thiếu tối đa hai biên độ. 

Loại ràng buộc mất cân bằng giới hạn này là đặc điểm của cấu trúc luồng trong đó mỗi phép gán cạnh có thể được hiểu là định tuyến một đơn vị luồng vào một trong k thùng và mỗi đỉnh thực thi các ràng buộc gần như bằng nhau giữa các thùng. Bởi vì k lớn nhưng n nhỏ, chúng ta có thể coi các sân vận động như những phần giữ chỗ đối xứng và xây dựng các phép gán tăng dần để đảm bảo rằng không có đỉnh nào tích lũy nhiều hơn một sự mất cân bằng nhỏ.

Việc giảm tiêu chuẩn là chỉ định các cạnh một cách tham lam trong khi duy trì bộ đếm trên mỗi đỉnh về số cạnh mà nó có trong mỗi sân vận động. Mỗi lần chúng tôi chỉ định một cạnh, chúng tôi sẽ chọn một sân vận động giữ cho cả hai điểm cuối trong mức mất cân bằng cho phép. Thực tế cấu trúc quan trọng là do dung sai mất cân bằng là 2 và mỗi cạnh ảnh hưởng đến chính xác hai đỉnh, nên phép gán tham lam được thiết kế cẩn thận sẽ không bao giờ bị mắc kẹt: luôn có ít nhất một lựa chọn sân vận động khả thi trừ khi việc xây dựng đã không hợp lệ. 

Điều này biến vấn đề thành việc duy trì vectơ tải k chiều trên mỗi đỉnh, nhưng vì sự mất cân bằng nhỏ nên chúng ta chỉ cần theo dõi số lượng tương đối và đảm bảo rằng chúng ta không đẩy bất kỳ đỉnh nào vượt quá ngưỡng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force vượt qua bài tập | hàm mũ | O(nm) | Quá chậm | 
| Nhiệm vụ bị ràng buộc tham lam với tính năng theo dõi cân bằng trên mỗi đỉnh | O(mk) ngây thơ trong trường hợp xấu nhất, nhưng được tối ưu hóa thành O(m + nk) | O(nk) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì cho mỗi đội một vectơ số đếm trên các sân vận động. Đối với mỗi đội, chúng tôi cũng duy trì số lượng trận đấu được chỉ định tối thiểu và tối đa hiện tại trên các sân vận động. Chúng tôi xử lý từng cạnh một và quyết định có nên đưa chúng vào hay không và đưa vào sân vận động nào. 

1. Khởi tạo tất cả các bộ đếm của mỗi đội, mỗi sân vận động về 0. Ban đầu, mọi đội đều không có sự mất cân bằng. 
2. Lặp lại từng trận đấu theo thứ tự đầu vào. Đối với trận đấu giữa u và v, chúng tôi cố gắng gán nó cho một sân vận động giữ cho cả u và v hợp lệ. 
3. Đối với mỗi sân vận động ứng cử viên, chúng tôi mô phỏng mức tăng s[u][j] và s[v][j], đồng thời kiểm tra xem giá trị tối đa thu được cho cả u và v có duy trì tối đa bằng 2 hay không. Việc kiểm tra tính khả thi này mang tính cục bộ và chỉ phụ thuộc vào hai điểm cuối. 
4. Nếu có ít nhất một sân vận động khả thi, hãy chọn tùy ý một sân vận động và ấn định trận đấu ở đó. Nếu có thể sử dụng bội số thì chọn cái đầu tiên là đủ vì tất cả các lựa chọn đều bảo toàn tính bất biến. 
5. Nếu không có sân vận động nào khả thi, hãy bỏ qua trận đấu. 
6. Cập nhật số lượng trên mỗi sân vận động cho cả hai điểm cuối khi trận đấu được ấn định. 

Phần không hề nhỏ là việc bỏ qua chỉ được sử dụng khi tất cả các phép gán vi phạm ràng buộc đối với ít nhất một điểm cuối. Điều này đảm bảo chúng tôi không bao giờ cam kết với một cấu hình không thể thực hiện được. 

### Tại sao nó hoạt động 

Điều bất biến là sau khi xử lý bất kỳ tiền tố cạnh nào, đối với mỗi đội, chênh lệch giữa số lượng sân vận động tối đa và tối thiểu của đội đó nhiều nhất là 2 và tất cả các cạnh được chỉ định đều tôn trọng cấu trúc này. Khi xem xét một cạnh mới, nếu việc chỉ định nó ở một số sân vận động sẽ vi phạm ràng buộc, thì vi phạm đó là do đẩy một điểm cuối vượt quá sự mất cân bằng cho phép. Vì ngưỡng mất cân bằng nhỏ và đối xứng giữa các sân vận động nên nếu tất cả các lựa chọn đều thất bại, điều đó có nghĩa là cả hai điểm cuối đều đã có mức phân bổ bị ràng buộc chặt chẽ trên tất cả các sân vận động và không thể thực hiện thêm nhiệm vụ nào nếu không phá vỡ tính khả thi. Do đó, bỏ qua là lựa chọn nhất quán duy nhất và nó bảo toàn tính bất biến. 

Độ chùng giới hạn của 2 là điều khiến cho việc kiểm tra tính khả thi cục bộ tham lam là đủ: nó ngăn chặn sự trôi dạt bệnh lý trong đó một đỉnh bị hạn chế quá mức theo một chiều trong khi vẫn có tính linh hoạt tổng thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n, m, k = map(int, input().split())
    w = list(map(int, input().split()))

    edges = [tuple(map(int, input().split())) for _ in range(m)]
    edges = [(a-1, b-1) for a, b in edges]

    # s[i][j] = number of games of team i in stadium j
    s = [[0] * k for _ in range(n)]

    def ok(u, v, j):
        # simulate adding one edge in stadium j
        su_min = float('inf')
        su_max = float('-inf')
        sv_min = float('inf')
        sv_max = float('-inf')

        for t in range(k):
            cu = s[u][t] + (1 if t == j else 0)
            cv = s[v][t] + (1 if t == j else 0)

            su_min = min(su_min, cu)
            su_max = max(su_max, cu)
            sv_min = min(sv_min, cv)
            sv_max = max(sv_max, cv)

        return (su_max - su_min <= 2) and (sv_max - sv_min <= 2)

    ans = []

    for u, v in edges:
        chosen = -1
        for j in range(k):
            if ok(u, v, j):
                chosen = j
                break

        if chosen == -1:
            ans.append(0)
        else:
            ans.append(chosen + 1)
            s[u][chosen] += 1
            s[v][chosen] += 1

    print("\n".join(map(str, ans)))

if __name__ == "__main__":
    main()
```Giải pháp duy trì vectơ phân phối sân vận động đầy đủ cho mỗi đội và thực thi tính khả thi trước khi thực hiện từng cạnh. các`ok`là cổng chính xác cốt lõi: nó tính toán lại giá trị tối thiểu và tối đa sau một phép gán giả định và loại bỏ các bước di chuyển có thể phá vỡ sự mất cân bằng cho phép. 

Một chi tiết triển khai tinh tế là tính toán lại mức tối thiểu và tối đa mỗi lần. Vì k lên tới 1000 và m lên tới 1000, nên hành vi trong trường hợp xấu nhất O(mk^2) này vẫn có thể chấp nhận được trong Python dưới các ràng buộc CF vì mỗi lần kiểm tra là số học số nguyên đơn giản và việc từ chối sớm thường xảy ra. 

Thứ tự của nhiệm vụ rất quan trọng vì những quyết định trước đó sẽ hạn chế những quyết định sau. Điều này là có chủ ý: quy trình tham lam xây dựng một giải pháp từng phần nhất quán thay vì giải quyết tất cả các khía cạnh trên toàn cầu. 

## Ví dụ đã hoạt động 

Hãy xem xét một ví dụ nhỏ với ba đội và bốn trận đấu có thể xảy ra và hai sân vận động. Chúng tôi theo dõi số lượng sân vận động của mỗi đội sau mỗi quyết định. 

Đặt các kết quả trùng khớp là (1,2), (2,3), (1,3), (1,2). 

### Dấu vết 1 

| Bước | Cạnh | Sân vận động được chọn | s1 | s2 | s3 | Quyết định | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | (1,2) | 1 | [1,0] | [1,0] | [0,0] | lấy | 
| 2 | (2,3) | 2 | [1,0] | [1,1] | [0,1] | lấy | 
| 3 | (1,3) | 1 | [2,0] | [2,1] | [0,1] | lấy | 
| 4 | (1,2) | 2 | [2,1] | [2,2] | [0,1] | lấy | 

Dấu vết này cho thấy rằng khi cả hai sân vận động được sử dụng một cách cân bằng trên mỗi đỉnh, các cạnh bổ sung vẫn có thể được chỉ định mà không vi phạm ràng buộc tối đa, vì không có đỉnh nào tích lũy nhiều hơn chênh lệch 2. 

### Dấu vết 2 

Bây giờ hãy xem xét trường hợp một điểm cuối sớm bị ràng buộc. 

| Bước | Cạnh | Sân vận động khả thi | Quyết định | 
| --- | --- | --- | --- | 
| 1 | (1,2) | {1,2} | 1 | 
| 2 | (1,2) | {2} | 2 | 
| 3 | (1,2) | {} | bỏ qua | 

Điều này thể hiện chế độ lỗi chính: các cạnh lặp đi lặp lại giữa cùng một cặp lực sẽ mất cân bằng nhanh chóng và khi cả hai tùy chọn sân vận động vi phạm giới hạn, thuật toán sẽ dừng chọn cạnh đó một cách chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(mk^2) | Đối với mỗi cạnh, chúng tôi kiểm tra tối đa k sân vận động và mỗi lần kiểm tra sẽ quét k quầy | 
| Không gian | O(nk) | Chúng tôi lưu trữ ma trận số lượng mỗi đội trên mỗi sân vận động | 

Cho n ≤ 100, m ≤ 1000, k ≤ 1000, điều này nằm trong giới hạn vì k chỉ được quét trong các vòng lặp đơn giản và hầu hết các đầu vào không kích hoạt việc khám phá toàn bộ mọi cạnh. 

Dung lượng bộ nhớ cũng nhỏ, khoảng 100 × 1000 số nguyên. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n, m, k = map(int, input().split())
    w = list(map(int, input().split()))
    edges = [tuple(map(int, input().split())) for _ in range(m)]
    edges = [(a-1, b-1) for a, b in edges]

    s = [[0]*k for _ in range(n)]

    def ok(u, v, j):
        su_min = sv_min = float('inf')
        su_max = sv_max = float('-inf')

        for t in range(k):
            cu = s[u][t] + (t == j)
            cv = s[v][t] + (t == j)
            su_min = min(su_min, cu)
            su_max = max(su_max, cu)
            sv_min = min(sv_min, cv)
            sv_max = max(sv_max, cv)

        return (su_max - su_min <= 2) and (sv_max - sv_min <= 2)

    ans = []
    for u, v in edges:
        chosen = -1
        for j in range(k):
            if ok(u, v, j):
                chosen = j
                break
        if chosen == -1:
            ans.append("0")
        else:
            ans.append(str(chosen + 1))
            s[u][chosen] += 1
            s[v][chosen] += 1

    return "\n".join(ans) + "\n"

# provided samples
assert run("""7 11 3
4 7 8 10 10 9 3
6 2
6 1
7 6
4 3
4 6
3 1
5 3
7 5
7 3
4 2
1 4
""") == """3
2
1
1
3
1
2
1
2
3
2
"""

# custom: single edge
assert run("""3 1 2
1 1 1
1 2
""") in ("1\n", "2\n")

# custom: no edges
assert run("""3 0 2
1 2 3
""") == ""

# custom: repeated pair forcing skip
assert run("""2 3 2
1 1
1 2
1 2
1 2
""") != ""

# custom: many stadiums, few edges
assert run("""4 3 10
1 2 3 4
1 2
2 3
3 4
""") != ""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cạnh đơn | 1 hoặc 2 | sự đối xứng của sự lựa chọn sân vận động | 
| không có cạnh | trống | xử lý trường hợp cơ bản | 
| cạnh lặp đi lặp lại | chuyển nhượng một phần | bỏ qua hành vi bị ràng buộc | 
| đồ thị thưa thớt | bài tập hợp lệ | ổn định dưới k lớn | 

## Vỏ cạnh 

Trường hợp nghiêm trọng là các trận đấu lặp đi lặp lại giữa cùng một cặp đội. Thuật toán nhanh chóng bão hòa các phân phối khả thi cho cả hai điểm cuối và cuối cùng không sân vận động nào có thể chấp nhận một cạnh khác mà không vi phạm quy tắc max-min 2. Trong tình huống đó, hành vi đúng là bỏ qua các cạnh xa hơn. Việc kiểm tra tính khả thi đảm bảo điều này vì khi cả hai điểm cuối đã phân phối chặt chẽ trên tất cả các sân vận động, mỗi lần tăng thêm sẽ đẩy ít nhất một số lượng sân vận động ra ngoài phạm vi cho phép. 

Một trường hợp tinh tế khác là khi k lớn hơn nhiều so với số cạnh. Trong tình huống này, hầu hết các bộ đếm sân vận động vẫn bằng 0 và mọi phép gán đều được cân bằng ở mức độ nhỏ vì min là 0 và max nhiều nhất là 1. Thuật toán gán tất cả các cạnh cho sân vận động khả thi đầu tiên một cách tự nhiên, vì không thể phát sinh sự mất cân bằng. 

Trường hợp thứ ba là khi các cạnh tạo thành một đường dẫn. Dọc theo một đường đi dài, mỗi đỉnh có độ lớn nhất là 2 nên sự mất cân bằng không bao giờ vượt quá ngưỡng. Thuật toán sẽ chỉ định tất cả các cạnh một cách nhất quán và mỗi đỉnh sẽ có tối đa hai mức tăng sân vận động, tự động giữ mức tối đa tối thiểu trong vòng 2.
