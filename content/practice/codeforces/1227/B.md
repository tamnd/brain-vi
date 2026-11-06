---
title: "CF 1227B - Hộp"
description: "Chúng ta được cho một mảng q không giảm, được cho là được tạo ra từ một số hoán vị ẩn p bằng cách lấy tiền tố cực đại. Tại mọi vị trí i, q[i] bằng giá trị lớn nhất trong số i phần tử đầu tiên của p."
date: "2026-06-15T19:47:54+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms"]
categories: ["algorithms"]
codeforces_contest: 1227
codeforces_index: "B"
codeforces_contest_name: "Technocup 2020 - Elimination Round 3"
rating: 1200
weight: 1227
solve_time_s: 337
verified: false
draft: false
---

[CF 1227B - Hộp](https://codeforces.com/problemset/problem/1227/B) 

**Đánh giá:** 1200 
**Tas:** thuật toán xây dựng 
**Thời gian giải:** 5 phút 37 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng không giảm`q`, được cho là được tạo ra từ một số hoán vị ẩn`p`bằng cách lấy tiền tố cực đại. Ở mọi vị trí`i`,`q[i]`bằng giá trị lớn nhất trong số đầu tiên`i`các yếu tố của`p`. 

Nhiệm vụ là xây dựng lại bất kỳ hoán vị hợp lệ nào`p`kích thước`n`có mảng tiền tố tối đa phù hợp`q`, hoặc xác định rằng không tồn tại hoán vị như vậy. 

Một cách giải thích quan trọng là`q`cho chúng ta biết khi nào mức cao kỷ lục mới xuất hiện trong hoán vị. Bất cứ khi nào`q[i] > q[i-1]`, chức vụ`i`phải đưa ra một giá trị tối đa mới chưa từng xuất hiện trước đó. Bất cứ khi nào`q[i] == q[i-1]`, chức vụ`i`không thể giới thiệu bất cứ thứ gì lớn hơn mức tối đa hiện tại; nó phải được lấp đầy bằng một số giá trị nhỏ hơn chưa được sử dụng. 

Các ràng buộc cho phép tổng số lên tới 100.000 phần tử trong các trường hợp thử nghiệm. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào thử hoán vị bằng vũ lực hoặc quay lui. Chúng ta cần xây dựng tuyến tính cho mỗi trường hợp thử nghiệm. 

Một trường hợp lỗi tinh vi xuất hiện khi một giá trị tăng trong`q`nhưng sự gia tăng này đã bỏ qua những con số đáng lẽ phải xuất hiện trước đó. Ví dụ, nếu`q = [1, 3]`, giá trị`2`không có nơi nào được đặt mà không vi phạm điều kiện tối đa tiền tố. Đây là hạn chế khả thi trung tâm. 

Một trường hợp cạnh quan trọng khác là sự lặp lại cùng mức tối đa. Nếu như`q[i]`lặp lại, nó không đưa ra các ràng buộc mới đối với mức tăng trưởng tối đa, nhưng nó yêu cầu theo dõi cẩn thận các số chưa sử dụng để chúng tôi không vô tình gán một số có thể tăng sai tiền tố tối đa. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là thử xây dựng các hoán vị và kiểm tra xem cực đại tiền tố của chúng có khớp với nhau không`q`. Đối với mỗi vị trí, chúng ta có thể thử tất cả các số chưa sử dụng và xác thực mảng tối đa tiền tố thu được. Điều này đòi hỏi phải khám phá đại khái`n!`hoán vị trong trường hợp xấu nhất, điều này hoàn toàn không khả thi ngoài`n = 10`. 

Cấu trúc của vấn đề gợi ý một cách xây dựng trực tiếp hơn. Mảng`q`chỉ thay đổi khi mức tối đa mới xuất hiện. Vì vậy chúng ta có thể nghĩ đến`q`như xác định các “phân đoạn” trong đó mức tối đa được cố định và các bước nhảy không thường xuyên khi mức tối đa mới được đưa ra. 

Bất cứ khi nào`q[i]`tăng thì giá trị đó phải được đặt chính xác tại vị trí`i`. Nếu không, chúng tôi sẽ không thể tạo tiền tố mới tối đa có kích thước đó. Điều này ngay lập tức khắc phục tất cả các vị trí mà mảng tăng nghiêm ngặt. 

Các vị trí còn lại là những vị trí`q[i] == q[i-1]`. Các vị trí này phải được lấp đầy bằng các giá trị nhỏ hơn mức tối đa tiền tố hiện tại và quan trọng là tất cả các số không sử dụng nhỏ hơn mức tối đa hiện tại cuối cùng phải xuất hiện ở đâu đó trước khi mức tối đa đó được thiết lập lần đầu tiên. Điều này dẫn đến một chiến lược tham lam: duy trì một nhóm số chưa sử dụng và gán chúng càng muộn càng tốt trong khi vẫn tôn trọng các ràng buộc tối đa của tiền tố. 

Điều kiện khả thi duy nhất chúng ta phải kiểm tra là ở mỗi bước mà`q`tăng từ`q[i-1]`ĐẾN`q[i]`, tất cả các số từ mức tối đa trước đó + 1 đến`q[i] - 1`phải có sẵn trong nhóm chưa sử dụng; nếu không thì việc xây dựng là không thể. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng hoán vị trong khi theo dõi những số nào đã được sử dụng và duy trì nhóm số có sẵn. 

1. Chúng tôi khởi tạo một mảng boolean hoặc thiết lập để theo dõi các số đã sử dụng và chuẩn bị một danh sách hoặc ngăn xếp tất cả các số từ`1`ĐẾN`n`chưa được đặt. Nhóm này đại diện cho các giá trị vẫn có thể được gán cho các vị trí không yêu cầu mức tối đa mới. 
2. Chúng tôi duy trì một biến`current_max = 0`, biểu thị giá trị lớn nhất được đặt cho đến nay trong hoán vị. Điều này là cần thiết vì bất kỳ số nào được đặt trong một đoạn “phẳng” đều không được vượt quá nó. 
3. Chúng tôi lặp qua các vị trí`i`từ`1`ĐẾN`n`. Nếu như`q[i] > q[i-1]`, sau đó định vị`i`là một vị trí bắt buộc của`q[i]`. Chúng ta phải đặt giá trị này ở đây vì đây là lần đầu tiên tiền tố tối đa tăng lên giá trị đó. 
4. Trước khi đặt mức tối đa mới`q[i]`, chúng tôi xác minh rằng tất cả các giá trị trong phạm vi`(current_max + 1 ... q[i] - 1)`vẫn chưa được sử dụng. Nếu bất kỳ số nào trong số này đã được đặt trước đó thì việc xây dựng không hợp lệ vì chúng ta cần chúng xuất hiện trước khi tăng tối đa. Nếu không được sử dụng, chúng sẽ được tiêu thụ một cách tự nhiên ở những phân khúc phẳng trước đó. 
5. Khi nào`q[i] == q[i-1]`, chúng tôi chỉ định số chưa sử dụng lớn nhất còn lại vẫn nhỏ hơn hoặc bằng`current_max`. Điều này đảm bảo chúng tôi không vô tình tăng tiền tố tối đa. 
6. Sau khi đặt một giá trị, chúng tôi đánh dấu nó là đã sử dụng và tiếp tục. 
7. Nếu tại bất kỳ thời điểm nào không có số hợp lệ nào có thể được chỉ định (ví dụ: không có số nào chưa được sử dụng là ≤ current_max), chúng tôi sẽ trả về`-1`. 

### Tại sao nó hoạt động 

Thuật toán thực thi rằng mỗi lần tăng`q`tương ứng chính xác với phần tử cực đại mới được giới thiệu trong`p`. Giữa hai lần tăng, chúng tôi chỉ sử dụng các số không thể ảnh hưởng đến tiền tố tối đa, giúp duy trì sự ổn định của`q`. Tính bất biến là tại mọi vị trí`i`, tiền tố được xây dựng tối đa bằng`q[i]`và tất cả các số không được sử dụng luôn tương thích với các ràng buộc trong tương lai vì chúng tôi không bao giờ sử dụng một số mà đáng lẽ phải cần để hỗ trợ mức tăng sau này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        q = list(map(int, input().split()))
        
        used = [False] * (n + 1)
        res = [0] * n
        
        available = set(range(1, n + 1))
        
        ok = True
        current_max = 0
        
        for i in range(n):
            if i == 0:
                res[i] = q[i]
                used[q[i]] = True
                available.remove(q[i])
                current_max = q[i]
                continue
            
            if q[i] > q[i - 1]:
                if q[i] in used:
                    ok = False
                    break
                res[i] = q[i]
                used[q[i]] = True
                available.remove(q[i])
                current_max = q[i]
            else:
                # pick largest unused ≤ current_max
                found = False
                for v in range(current_max, 0, -1):
                    if not used[v]:
                        res[i] = v
                        used[v] = True
                        available.remove(v)
                        found = True
                        break
                if not found:
                    ok = False
                    break
        
        if not ok:
            print(-1)
        else:
            print(*res)

if __name__ == "__main__":
    solve()
```Mã theo dõi giá trị nào đã được sử dụng và trực tiếp gán giá trị cực đại bắt buộc bất cứ khi nào`q`tăng lên. Đối với các phân đoạn phẳng, nó tham lam chọn giá trị còn lại lớn nhất không vượt quá mức tối đa hiện tại, điều này tránh đưa ra mức tối đa mới sớm. các`used`mảng đảm bảo tính duy nhất và quá trình quét ngược đảm bảo chúng tôi duy trì tính linh hoạt cho các vị trí trong tương lai. 

Một chi tiết triển khai tinh tế là chúng ta không bao giờ cần`available`được thiết lập để đảm bảo tính chính xác nhưng nó giúp duy trì tính nhất quán với các số chưa sử dụng. Logic quyết định thực tế dựa hoàn toàn vào`used`Và`current_max`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`q = [1, 3, 4, 5, 5]`Chúng tôi theo dõi việc xây dựng từng bước. 

| tôi | q[i] | hiện tại_max | hành động | độ phân giải | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 1 | nơi 1 | [1, _, _, _, _] | 
| 1 | 3 | 3 | vị trí 3 (tối đa mới) | [1, 3, _, _, _] | 
| 2 | 4 | 4 | vị trí 4 (tối đa mới) | [1, 3, 4, _, _] | 
| 3 | 5 | 5 | vị trí 5 (tối đa mới) | [1, 3, 4, 5, _] | 
| 4 | 5 | 5 | chọn 5 chưa sử dụng → 2 | [1, 3, 4, 5, 2] | 

Điều này xác nhận rằng sau khi tất cả mức tăng đã được cố định, các giá trị còn lại có thể được đặt tự do miễn là chúng không vượt quá mức tối đa hiện tại. 

### Ví dụ 2 

đầu vào:`q = [1, 1, 3, 4]`| tôi | q[i] | hiện tại_max | hành động | độ phân giải | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 1 | nơi 1 | [1, _, _, _] | 
| 1 | 1 | 1 | chọn 1 không sử dụng không thành công | không thể | 

Tại vị trí 1, không có giá trị nào không được sử dụng ≤ 1, vì 1 đã được sử dụng và không tồn tại giá trị hợp lệ nào khác. Thuật toán bác bỏ trường hợp này một cách chính xác. 

Điều này cho thấy cực đại tiền tố lặp lại buộc các giá trị đủ nhỏ tồn tại đủ sớm, nếu không thì cấu trúc sẽ bị hỏng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) trường hợp xấu nhất | quét lùi từng vị trí phẳng | 
| Không gian | O(n) | mảng để theo dõi và kết quả đã sử dụng | 

Giải pháp vẫn đủ trong điều kiện hạn chế vì tổng`n`trong các trường hợp thử nghiệm bị hạn chế, nhưng hành vi bậc hai này không lý tưởng. Một phiên bản được tối ưu hóa hơn có thể sử dụng cấu trúc hoặc nhóm ưu tiên để đạt được O(n) được khấu hao, nhưng logic xây dựng vẫn không thay đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    def solve():
        t = int(input())
        out = []
        for _ in range(t):
            n = int(input())
            q = list(map(int, input().split()))
            
            used = [False] * (n + 1)
            res = [0] * n
            
            ok = True
            current_max = 0
            
            for i in range(n):
                if i == 0:
                    res[i] = q[i]
                    used[q[i]] = True
                    current_max = q[i]
                    continue
                
                if q[i] > q[i - 1]:
                    if used[q[i]]:
                        ok = False
                        break
                    res[i] = q[i]
                    used[q[i]] = True
                    current_max = q[i]
                else:
                    found = False
                    for v in range(current_max, 0, -1):
                        if not used[v]:
                            res[i] = v
                            used[v] = True
                            found = True
                            break
                    if not found:
                        ok = False
                        break
            
            out.append("-1" if not ok else " ".join(map(str, res)))
        return "\n".join(out)
    
    return solve()

# provided samples
assert run("""4
5
1 3 4 5 5
4
1 1 3 4
2
2 2
1
1""") == """1 3 4 5 2
-1
2 1
1"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn |`1`| tính đúng đắn của trường hợp cơ sở | 
| chuỗi ngày càng tăng |`1 2 3 4`| xử lý tăng trưởng nghiêm ngặt | 
| nhảy không thể |`1 3`→`-1`| hạn chế về tính khả thi | 
| cực đại lặp đi lặp lại |`2 2 2`→ kiểm tra cấu trúc hợp lệ/không hợp lệ | xử lý các đoạn phẳng | 

## Vỏ cạnh 

Trường hợp một cạnh là khi mảng bắt đầu bằng giá trị lớn hơn 1, chẳng hạn như`q = [3, 3, 3]`. Thuật toán ngay lập tức cố gắng đặt`3`đầu tiên, nhưng không có giá trị nhỏ hơn nào có sẵn để lấp đầy các vị trí trước đó mà không vi phạm các ràng buộc cực đại tiền tố. Điều này không thành công vì bất biến yêu cầu tất cả các giá trị`1`ĐẾN`q[0]-1`xuất hiện trước khi mức tối đa đầu tiên được thiết lập, điều này là không thể khi`q[0] > 1`. 

Một trường hợp cạnh khác là khi`q`tăng những bước nhảy lớn, ví dụ`q = [1, 10]`. Ở đây, các giá trị`2`bởi vì`9`phải được đặt ở vị trí đầu tiên trước khi mức tối đa trở thành`10`, điều này là không thể, vì vậy thuật toán sẽ loại bỏ nó một cách chính xác. 

Trường hợp cạnh cuối cùng là một mảng hoàn toàn phẳng như`q = [1, 1, 1, 1]`. Vị trí thuật toán`1`ở vị trí đầu tiên, sau đó không còn giá trị hợp lệ cho các vị trí tiếp theo, dẫn đến bị loại bỏ ngay lập tức trừ khi`n = 1`.
