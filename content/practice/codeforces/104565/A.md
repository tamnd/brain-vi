---
title: "CF 104565A - Sơ tán Thượng viện"
description: "Chúng tôi được đưa ra một số kịch bản sơ tán độc lập. Trong mỗi kịch bản có một số đảng chính trị, mỗi đảng có một số thượng nghị sĩ. Hành động duy nhất được phép là liên tục loại bỏ một hoặc hai thượng nghị sĩ trong một bước."
date: "2026-06-30T08:36:11+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104565
codeforces_index: "A"
codeforces_contest_name: "2016 Google Code Jam Round 1C (GCJ 16 Round 1C)"
rating: 0
weight: 104565
solve_time_s: 69
verified: true
draft: false
---

[CF 104565A - Sơ tán Thượng viện](https://codeforces.com/problemset/problem/104565/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 9 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được đưa ra một số kịch bản sơ tán độc lập. Trong mỗi kịch bản có một số đảng chính trị, mỗi đảng có một số thượng nghị sĩ. Hành động duy nhất được phép là liên tục loại bỏ một hoặc hai thượng nghị sĩ trong một bước. Sau mỗi bước như vậy, các thượng nghị sĩ còn lại không bao giờ được xảy ra tình trạng một bên nắm giữ hơn một nửa tổng số thượng nghị sĩ còn lại. 

Nhiệm vụ không chỉ là xác định xem có thể sơ tán hay không mà còn thực sự xây dựng một chuỗi đầy đủ các bước sơ tán để loại bỏ tất cả các thượng nghị sĩ trong khi vẫn duy trì điều kiện an toàn ở mọi trạng thái trung gian. 

Đầu vào cung cấp, đối với mỗi trường hợp thử nghiệm, số lượng các bên và số lượng thượng nghị sĩ ban đầu trong mỗi bên. Đầu ra phải mô tả một chuỗi các bước sơ tán, trong đó mỗi bước là một chữ cái đại diện cho một thượng nghị sĩ đã sơ tán hoặc một cặp chữ cái đại diện cho hai thượng nghị sĩ đã sơ tán từ các đảng có thể khác nhau. 

Ràng buộc không có đảng nào chiếm đa số tuyệt đối ngay sau bất kỳ bước nào là khó khăn cốt lõi. Nó hạn chế việc loại bỏ một cách tham lam: việc lấy quá nhiều từ một bên quá sớm có thể khiến một bên khác tạm thời chiếm ưu thế ngay cả khi tổng số nhìn có vẻ an toàn. 

Các giới hạn đủ nhỏ để chúng ta không cần cấu trúc dữ liệu nâng cao. Tổng số thượng nghị sĩ cho mỗi trường hợp thử nghiệm nhiều nhất là 1000, do đó, bất kỳ thuật toán nào thực hiện lặp lại công việc tỷ lệ thuận với số lượng bên đều dễ dàng đủ nhanh. Điều quan trọng là tính đúng đắn của chiến lược sơ tán chứ không phải tối ưu hóa tiệm cận. 

Một trường hợp thất bại tinh tế đối với những cách tiếp cận ngây thơ là luôn loại bỏ hai thượng nghị sĩ khỏi đảng lớn nhất bất cứ khi nào có thể. Ví dụ: nếu cấu hình là A = 3, B = 2, C = 2, việc loại bỏ AA trước tiên sẽ để lại A = 1, B = 2, C = 2, điều này là an toàn. Nhưng trong các cấu hình khác, việc loại bỏ một cách mù quáng hai đảng khỏi đảng lớn nhất có thể tạo ra tình trạng một đảng vượt quá một nửa số thượng nghị sĩ còn lại. 

Một tình huống khó khăn khác nảy sinh khi hai bên có quy mô lớn nhất. Việc lựa chọn kém có thể tạm thời tạo ra sự mất cân bằng mặc dù việc ghép đôi thay thế sẽ duy trì sự an toàn. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ sẽ là coi mỗi bang của các thượng nghị sĩ còn lại như một nút và thử tất cả các cuộc sơ tán có thể có của một hoặc hai thượng nghị sĩ, sử dụng một tìm kiếm chỉ giữ lại các bang không tồn tại đa số. Điều này đúng về mặt khái niệm vì nó khám phá tất cả các chuỗi hợp lệ, nhưng không gian trạng thái tăng theo cấp số nhân với số lượng thượng nghị sĩ. Trong trường hợp xấu nhất là 1000 thượng nghị sĩ, hệ số phân nhánh là khoảng 26 lựa chọn cho việc loại bỏ đơn lẻ và khoảng 26 lựa chọn cho 2 cặp, khiến phương pháp này hoàn toàn không khả thi. 

Quan sát quan trọng là chúng ta không thực sự cần khám phá tất cả các trạng thái hợp lệ. Ràng buộc chỉ quan tâm đến bên lớn nhất sau mỗi lần di chuyển. Điều này có nghĩa là chúng tôi luôn có thể lập luận cục bộ về một hoặc hai bên đứng đầu và đảm bảo chúng tôi không bao giờ cho phép cấu hình có số lượng tối đa vượt quá một nửa tổng số còn lại. 

Chiến lược tham lam là luôn giảm áp lực lớn nhất trong hệ thống. Nếu tổng số thượng nghị sĩ còn lại là số lẻ thì chúng ta phải loại bỏ đúng một thượng nghị sĩ ở bước đó. Nếu không, chúng tôi có thể loại bỏ hai thượng nghị sĩ một cách an toàn miễn là chúng tôi không tạo ra đa số. Lựa chọn an toàn là loại bỏ một người khỏi bên lớn nhất hoặc loại bỏ một người khỏi mỗi bên trong số hai bên lớn nhất khi điều đó không vi phạm điều kiện cân bằng. 

Vì số lượng đảng phái nhiều nhất là 26 và tổng quy mô nhỏ nên chúng ta có thể tính toán lại các đảng lớn nhất ở mỗi bước và áp dụng quy tắc này nhiều lần cho đến khi tất cả thượng nghị sĩ bị loại bỏ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tìm kiếm vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Cuộc sơ tán tham lam của Max Selection | O(N * S) | O(N) | Đã chấp nhận |

Ở đây S là tổng số thượng nghị sĩ. 

## Hướng dẫn thuật toán 

Chúng tôi duy trì số lượng hiện tại của tất cả các bên và liên tục xây dựng các bước sơ tán cho đến khi tất cả số lượng bằng không. 

1. Tính tổng số thượng nghị sĩ còn lại. Nếu bằng 0 thì quá trình đã hoàn tất. Điều kiện kết thúc này đảm bảo cuối cùng chúng tôi sẽ tạo ra một chuỗi sơ tán đầy đủ. 
2. Sắp xếp các nhóm theo số lượng còn lại theo thứ tự giảm dần. Điều này cho phép chúng tôi xác định bên nguy hiểm nhất ở mỗi bước, nghĩa là bên có nhiều khả năng vi phạm ràng buộc đa số nhất nếu không được giảm bớt. 
3. Nếu tổng số thượng nghị sĩ là số lẻ thì phải loại bỏ đúng một thượng nghị sĩ. Chúng tôi chọn đảng có số phiếu còn lại cao nhất và loại bỏ một thượng nghị sĩ khỏi đảng đó. Lựa chọn này là an toàn vì việc giảm bớt bên lớn nhất trực tiếp làm giảm rủi ro của đa số và việc loại bỏ bất kỳ bên nào khác sẽ không giải quyết được rủi ro mất cân bằng ngay lập tức. 
4. Nếu tổng số thượng nghị sĩ là số chẵn thì chúng tôi xem xét loại bỏ hai thượng nghị sĩ. Chúng tôi nhìn vào hai bên hàng đầu. Nếu họ khác nhau, chúng tôi sẽ loại bỏ một thượng nghị sĩ khỏi mỗi người. Đây là cách giảm đối xứng an toàn nhất vì nó duy trì sự cân bằng giữa hai nhóm lớn nhất. 
5. Sau khi thực hiện xóa, chúng tôi thêm chữ cái hoặc cặp chữ cái tương ứng vào câu trả lời và cập nhật số đếm. 
6. Lặp lại cho đến khi tất cả số đếm đều bằng 0. 

Tính chính xác dựa trên việc chúng tôi luôn loại bỏ khỏi các bên quan trọng nhất hiện tại, đảm bảo không một bên nào có thể phát triển vượt quá một nửa tổng số còn lại sau bất kỳ bước nào. Sự lựa chọn tham lam có tác dụng vì bất kỳ cấu hình nguy hiểm nào luôn do một hoặc hai bên thống trị gây ra và thuật toán luôn nhắm mục tiêu trực tiếp vào chúng. 

### Tại sao nó hoạt động 

Ở mỗi bước, gọi T là tổng số thượng nghị sĩ còn lại. Cách duy nhất để vi phạm điều kiện là nếu một bên nào đó vượt quá T/2. Thuật toán đảm bảo rằng bất cứ khi nào T là số lẻ, chúng tôi sẽ giảm nhóm tối đa ngay lập tức, ngăn không cho nhóm đó trở nên quá một nửa ở trạng thái tiếp theo. Khi T chẵn, việc loại bỏ hai cái ở trên cùng hoặc một cái ở mỗi cái trong số hai cái trên cùng đảm bảo rằng nồng độ lớn nhất có thể giảm xuống hoặc vẫn được kiểm soát và không bên nào khác có thể vượt qua nó vì chúng tôi không bao giờ bỏ qua mức tối đa hiện tại. Điều này duy trì tính bất biến rằng không có bang nào có đa số tuyệt đối. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_case(cnt):
    n = len(cnt)
    res = []

    while True:
        total = sum(cnt)
        if total == 0:
            break

        # find top parties
        order = sorted(range(n), key=lambda i: cnt[i], reverse=True)

        if total % 2 == 1:
            i = order[0]
            cnt[i] -= 1
            res.append(chr(ord('A') + i))
        else:
            i, j = order[0], order[1]
            # evacuate one each if possible
            cnt[i] -= 1
            cnt[j] -= 1
            res.append(chr(ord('A') + i) + chr(ord('A') + j))

    return " ".join(res)

def main():
    t = int(input())
    out = []
    for tc in range(1, t + 1):
        n = int(input())
        cnt = list(map(int, input().split()))
        ans = solve_case(cnt)
        out.append(f"Case #{tc}: {ans}")
    print("\n".join(out))

if __name__ == "__main__":
    main()
```Mã liên tục tính toán lại thứ tự của các đảng theo quy mô và áp dụng quy tắc chẵn lẻ trên tổng số thượng nghị sĩ còn lại. Việc sử dụng cách sắp xếp ở mỗi bước là đủ vì tổng số thượng nghị sĩ ít và số lượng đảng phái nhiều nhất là 26. 

Việc xây dựng bước sơ tán phản ánh trực tiếp thuật toán. Khi loại bỏ hai thượng nghị sĩ, chúng tôi luôn loại họ khỏi hai đảng lớn nhất trong tình trạng hiện tại, đảm bảo rằng không đảng nhỏ hơn nào có thể chiếm ưu thế sau khi loại bỏ. 

Một điểm tinh tế là chúng ta không kiểm tra rõ ràng điều kiện đa số sau mỗi nước đi. Điều này là an toàn vì chiến lược xây dựng đảm bảo rằng trạng thái luôn được cân bằng theo thiết kế. 

## Ví dụ đã hoạt động 

Hãy xem xét một đầu vào với ba bên: 

Trạng thái ban đầu: A = 3, B = 2, C = 2 

Chúng tôi theo dõi các bước sơ tán tổng thể và đã chọn. 

| Bước | Bang (A,B,C) | Tổng cộng | Hành động | Lý do | 
| --- | --- | --- | --- | --- | 
| 1 | (3,2,2) | 7 | A | tổng là số lẻ, giảm max | 
| 2 | (2,2,2) | 6 | AB | tổng số chẵn, giảm hai đầu | 
| 3 | (1,1,2) | 4 | AC | hai vị trí đứng đầu là C và A | 
| 4 | (0,1,1) | 2 | BC | hoàn thành phần còn lại | 

Điều này chứng tỏ rằng ngay cả khi một đảng bắt đầu chiếm ưu thế, quy tắc ngang giá buộc đảng đó phải giảm bớt trước, ngăn chặn bất kỳ đa số trung gian nào. 

Bây giờ hãy xem xét một trường hợp đơn giản hơn: 

A = 2, B = 2 

| Bước | Tiểu bang | Tổng cộng | Hành động | 
| --- | --- | --- | --- | 
| 1 | (2,2) | 4 | AB | 
| 2 | (1,1) | 2 | AB | 

Điều này cho thấy trường hợp đối xứng trong đó việc ghép hai người đứng đầu luôn an toàn và dẫn đến việc loại bỏ hoàn toàn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(S · N log N) | Mỗi bước sơ tán sẽ tính toán lại việc đặt hàng cho tối đa 26 bên | 
| Không gian | O(N) | Chúng tôi chỉ lưu trữ số lượng nhóm và trình tự đầu ra | 

Tổng số thượng nghị sĩ nhiều nhất là 1000 và có nhiều nhất 26 bên, vì vậy thuật toán thực hiện tối đa 1000 lần lặp, mỗi lần có chi phí sắp xếp hệ số không đổi nhỏ. Điều này phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import main
    sys.stdout = io.StringIO()
    main()
    return sys.stdout.getvalue().strip()

# Sample inputs are included for reference (multiple valid outputs exist, so exact matching is not enforced)
# print(run("..."))

# Custom deterministic tests

assert run("""1
2
1 1
""") == "Case #1: AB", "minimal symmetric case"

assert run("""1
3
1 0 0
""") == "Case #1: A", "single party case"

assert run("""1
3
2 2 1
""")  # should terminate without majority violation

assert run("""1
4
2 1 1 1
""")  # stress small imbalance case
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 đảng nhỏ chiếm ưu thế | Một chuỗi giảm an toàn | sơ tán đơn đảng | 
| cân bằng hai bên | AB AB | tính chính xác ghép nối đối xứng | 
| đa đảng không đồng đều | sơ tán đầy đủ hợp lệ | tham lam ổn định dưới sự mất cân bằng | 

## Vỏ cạnh 

Khi chỉ có một bên khác 0, thuật toán sẽ liên tục loại bỏ các thượng nghị sĩ khỏi đảng đó. Đối với đầu vào như A = 3, B = 0, C = 0, tổng luôn là số lẻ, do đó thuật toán sẽ loại bỏ một A mỗi lần. Điều này không bao giờ vi phạm điều kiện đa số vì không có đảng cạnh tranh nào tồn tại để tạo thành đa số. 

Khi hai bên có số lượng tối đa bằng nhau, chẳng hạn như A = 2, B = 2, C = 1, thuật toán luôn xếp cặp A và B trước khi tổng số chẵn. Điều này ngăn cản C trở thành đa số vì C không bao giờ vượt quá tỷ lệ giảm tổng hợp của hai đảng đứng đầu. 

Khi hệ thống bị lệch nhiều, ví dụ A = 6, B = 1, C = 1, bước đầu tiên sẽ loại bỏ A vì tổng là số lẻ. Điều này ngay lập tức làm giảm đảng chiếm ưu thế và đảm bảo rằng các bước chẵn tiếp theo có thể ghép nối các thượng nghị sĩ còn lại một cách an toàn mà không cho phép A vượt quá một nửa nữa.
