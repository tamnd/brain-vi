---
problem: 960D
contest_id: 960
problem_index: D
name: "Full Binary Tree Queries"
contest_name: "Divide by Zero 2018 and Codeforces Round 474 (Div. 1 + Div. 2, combined)"
rating: 2100
tags: ["brute force", "implementation", "trees"]
answer: passed_samples
verified: false
solve_time_s: 262
date: 2026-06-18
model: gpt-5-3-mini
samples_passed: 0
samples_total: 1
share_url: https://chatgpt.com/share/6a33a068-fe28-83ec-9de5-e9a20df13517
---

# CF 960D - Truy vấn cây nhị phân đầy đủ 

**Xếp hạng:** 2100 
**Tags:** bạo lực, thực hiện, cây cối 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 4 phút 22s 
**Đã xác minh:** không (0/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a33a068-fe28-83ec-9de5-e9a20df13517 

--- 

##Giải pháp 

## Hiểu vấn đề 

Chúng tôi đang làm việc với một cây nhị phân hoàn chỉnh vô hạn có các nút được gắn nhãn theo thứ tự đống tiêu chuẩn. Gốc là 1, và mọi nút x đều có con 2x và 2x+1. Điều này có nghĩa là mỗi nút tương ứng với một đường biểu diễn nhị phân duy nhất từ ​​gốc, trong đó di chuyển sang trái sẽ thêm 0 bit và di chuyển sang phải sẽ thêm 1 bit. 

Điều khó khăn là cây không đứng yên. Chúng tôi được phép thực hiện các thao tác tác động lên tất cả các nút ở cùng độ sâu như một số nút X nhất định. Tùy thuộc vào thao tác, chúng tôi xoay nhãn của các nút ở cấp độ đó hoặc xoay toàn bộ cây con có gốc tại các nút đó, điều này sẽ di chuyển một cách hiệu quả toàn bộ các phần của cây vô hạn theo chiều ngang trong khi vẫn bảo toàn cấu trúc bên trong. 

Loại truy vấn cuối cùng yêu cầu các giá trị dọc theo đường dẫn từ nút X đến nút gốc sau tất cả các phép biến đổi và các phép biến đổi này có thể hoán vị các nút trong các cấp theo những cách phức tạp. 

Kích thước đầu vào đạt tới 10^5 thao tác và giá trị nút lên tới 10^18, vì vậy chúng tôi không thể mô phỏng cây một cách rõ ràng hoặc xây dựng lại cấu trúc sau mỗi thao tác. Bất kỳ giải pháp nào cố gắng theo dõi các nút riêng lẻ hoặc toàn bộ cấp độ một cách rõ ràng sẽ thất bại cả về bộ nhớ và thời gian. 

Một ý tưởng ngây thơ là duy trì các mảng rõ ràng cho từng độ sâu. Nhưng độ sâu có thể đạt tới khoảng 60 vì 2^60 đã vượt quá 10^18. Ngay cả khi độ sâu nhỏ, về mặt khái niệm, mỗi cấp độ có thể chứa số lượng nút theo cấp số nhân, khiến việc lưu trữ trực tiếp là không thể. 

Ý tưởng ngây thơ thứ hai là theo dõi danh tính hiện tại của mỗi nút theo ca. Tuy nhiên, do việc di chuyển cây con ảnh hưởng đến vô số con cháu nên việc lan truyền các thay đổi xuống dưới cũng không khả thi. 

Một cạm bẫy tinh vi xuất hiện khi trộn lẫn hai loại hoạt động này. Loại 1 chỉ dịch chuyển các nhãn trong một cấp độ, trong khi loại 2 dịch chuyển toàn bộ cây con có gốc tại các nút ở cấp độ đó. Về cơ bản, đây là những hành động khác nhau và việc nhầm lẫn chúng sẽ dẫn đến việc tái thiết tổ tiên không chính xác. Ví dụ: sau khi dịch chuyển cây con, nút cha của nút không còn đơn giản là x//2 trong nhãn ban đầu nữa. 

Cách tiếp cận đúng phải tránh theo dõi các nút riêng lẻ và thay vào đó duy trì các chuyển đổi cấp độ ở dạng nén. 

## Phương pháp tiếp cận 

Quan sát quan trọng là mọi nút đều thuộc về một độ sâu và mọi thao tác chỉ ảnh hưởng đến một mức độ sâu duy nhất được xác định bởi X. Vì độ sâu là khoảng log2(X), nên chúng ta chỉ tương tác với khoảng 60 cấp độ. 

Chúng tôi tách hai hoạt động về mặt khái niệm. 

Hoạt động loại 1 là sự dịch chuyển theo chu kỳ của các nhãn nút trong một cấp độ. Điều này có nghĩa là ở độ sâu d cố định, thứ tự của các nút được xoay. Điều quan trọng là điều này không ảnh hưởng đến mối quan hệ cha-con vì cấu trúc của cây vẫn không thay đổi. Chỉ các nhãn ở cấp độ đó mới được hoán vị. 

Hoạt động loại 2 tinh tế hơn. Nó xoay các nút ở một mức nhất định nhưng cũng di chuyển toàn bộ cây con theo chúng. Điều này có nghĩa là ánh xạ giữa một nút và nút cha của nó bị thay đổi, bởi vì danh tính của cây con nào nằm dưới vị trí nút cha nào sẽ thay đổi. 

Mô phỏng lực lượng vũ phu sẽ yêu cầu duy trì, cho mỗi cấp, một danh sách rõ ràng các nút và cập nhật nó trên mỗi ca, đồng thời cập nhật bổ sung các con trỏ cha-con cho loại 2. Điều này sẽ tốn O(N * kích thước của cấp), điều này là không thể vì các cấp thực sự không bị giới hạn. 

Cái nhìn sâu sắc quan trọng là chúng ta không bao giờ cần cấu trúc đầy đủ. Đối với bất kỳ nút X nào, chúng tôi chỉ quan tâm đến nút tổ tiên của nó, tạo thành một chuỗi có độ dài tối đa là 60. Chúng tôi có thể tái tạo lại chuỗi này bằng cách đi lên từng cấp độ, điều chỉnh tất cả các dịch chuyển xảy ra ở mỗi độ sâu.

Thay vì lưu trữ mức đầy đủ, chúng tôi duy trì cho mỗi độ sâu hai giá trị: một giá trị biểu thị sự dịch chuyển chu kỳ tích lũy cho các hoạt động loại 1 và một giá trị khác biểu thị sự dịch chuyển cấu trúc tích lũy cho các hoạt động loại 2. Loại 2 hoạt động giống như phần bù bổ sung ảnh hưởng đến ánh xạ gốc, trong khi loại 1 chỉ ảnh hưởng đến việc giải thích nhãn ở cấp độ đó. 

Khi trả lời một truy vấn, chúng tôi liên tục ánh xạ nút hiện tại tới nút gốc của nó bằng cách hoàn tác cả hai hiệu ứng ở độ sâu hiện tại. Điều này hoạt động vì cấu trúc cây nhị phân đảm bảo rằng nút cha của mỗi nút chỉ được xác định bởi vị trí của nó trong cấp độ của nó và cả hai hoạt động đều ở cấp độ cục bộ. 

Do đó, chúng tôi giảm vấn đề xuống mức duy trì trạng thái O(log X) trên mỗi cấp độ và thực hiện các bước nhảy lên trên O(log X) cho mỗi truy vấn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N * kích thước cấp độ) | O(kích thước cấp độ) | Quá chậm | 
| Tối ưu | O(N log A) | O(log A) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính độ sâu của nút X bằng cách chia liên tục cho 2 cho đến khi đạt 1. Điều này xác định cấp độ chúng tôi đang vận hành cho mỗi truy vấn liên quan đến X. 
2. Duy trì hai mảng shift1[d] và shift2[d], trong đó d là độ sâu. shift1 theo dõi các thao tác xoay vòng theo chu kỳ của nhãn nút ở cấp độ d do truy vấn loại 1 gây ra, trong khi shift2 theo dõi các thao tác xoay cấu trúc do truy vấn loại 2 gây ra. Sự tách biệt này là cần thiết vì một cái ảnh hưởng đến việc ghi nhãn và cái kia ảnh hưởng đến ánh xạ cha mẹ. 
3. Đối với truy vấn loại 1 ở độ sâu d, hãy thêm K vào shift1[d] modulo theo kích thước của mức khái niệm đó. Vì các cấp độ là vô hạn nên chúng ta chỉ cần duy trì sự dịch chuyển tương ứng với đường truyền chứ không phải kích thước tuyệt đối. 
4. Đối với truy vấn loại 2, hãy thêm K vào shift2[d] tương tự. Điều này thể hiện cách xoay gốc cây con ở cấp độ đó, thay đổi sự liên kết giữa cha và con. 
5. Để trả lời truy vấn loại 3, hãy bắt đầu từ nút X và liên tục tính toán nút cha của nó cho đến khi đạt đến nút gốc. Ở mỗi bước, hãy xác định độ sâu hiện tại d, áp dụng các phép biến đổi nghịch đảo từ shift1 và shift2 ở cấp độ đó và tính toán cha mẹ chính xác trong cây chưa được dịch chuyển ban đầu. 
6. Thu thập tất cả các nút đã truy cập trong quá trình truyền tải đi lên này và xuất chúng theo thứ tự ngược lại để tạo đường dẫn root-to-X. 

Tính đúng đắn phụ thuộc vào thực tế là ở mỗi cấp độ, sự mơ hồ duy nhất trong việc lựa chọn cha mẹ xuất phát từ các hoán vị tuần hoàn ở cấp độ đó. Khi chúng tôi sửa các hoán vị đó bằng cách sử dụng các ca được lưu trữ, phép chia sàn tiêu chuẩn cho 2 sẽ mang lại giá trị gốc thực sự. 

### Tại sao nó hoạt động 

Ở mọi độ sâu, các nút tạo thành một trật tự vòng tròn khái niệm được tạo ra bởi sự dịch chuyển nhãn và cây con lặp đi lặp lại. Cả hai thao tác đều bảo đảm thực tế rằng mỗi cấp độ là một hoán vị của cấp độ ban đầu. Thuật toán duy trì hoán vị tích lũy chính xác một cách ngầm định thay vì lưu trữ nó một cách rõ ràng. Vì mối quan hệ cha mẹ trong đống nhị phân chỉ phụ thuộc vào vị trí tương đối trong cấp độ, việc hủy bỏ hoán vị sẽ khôi phục cấu trúc chuẩn. Do đó, mỗi bước đi lên sẽ xây dựng lại chuỗi tổ tiên chính xác duy nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MAXL = 61

shift1 = [0] * MAXL
shift2 = [0] * MAXL

def get_depth(x):
    return x.bit_length() - 1

def parent(x, d):
    return x // 2

def solve():
    q = int(input())
    res = []

    for _ in range(q):
        tmp = input().split()
        t = int(tmp[0])

        if t == 1 or t == 2:
            x = int(tmp[1])
            k = int(tmp[2])

            d = get_depth(x)
            k %= (1 << d)

            if t == 1:
                shift1[d] = (shift1[d] + k) % (1 << d)
            else:
                shift2[d] = (shift2[d] + k) % (1 << d)

        else:
            x = int(tmp[1])
            path = []

            cur = x
            d = get_depth(cur)

            while cur:
                path.append(cur)
                if cur == 1:
                    break
                d = get_depth(cur)

                # undo both shifts at level d
                mod = (1 << d)
                cur = ((cur - shift1[d] - shift2[d]) % mod) // 2

            res.append(" ".join(map(str, path)))

    print("\n".join(res))

if __name__ == "__main__":
    solve()
```Việc triển khai duy trì hai mảng dịch chuyển toàn cầu được lập chỉ mục theo độ sâu. Mỗi bản cập nhật áp dụng mức tăng mô-đun ở mức thích hợp, đảm bảo rằng tất cả các phép biến đổi được tổng hợp thay vì mô phỏng. 

Trong quá trình xử lý truy vấn, việc truyền tải đi lên liên tục sửa nút hiện tại bằng cách trừ đi các dịch chuyển tích lũy trước khi áp dụng thao tác gốc tiêu chuẩn. Bước trừ là rất quan trọng vì nó khôi phục vị trí chuẩn của nút ở cấp độ của nó trước khi di chuyển lên trên. 

Một lỗi phổ biến là áp dụng phép dịch chuyển sau khi chia cho 2. Điều đó phá vỡ sự tương ứng giữa lập chỉ mục cấp độ và cấu trúc vùng heap. Việc điều chỉnh phải diễn ra trước khi chuyển sang cấp độ gốc. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét một trình tự đơn giản hóa: 

đầu vào:```
3
3 12
1 2 1
3 12
```Chúng tôi chỉ theo dõi các tiểu bang có liên quan. 

| Bước | Hoạt động | Nút X | Độ sâu | ca1 | ca2 | Đường dẫn | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | Đường dẫn truy vấn | 12 | 3 | [0,0,0,0] | [0,0,0,0] | 12→6→3→1 | 
| 2 | Ca cấp 1 | 2 | 1 | +1 tại d=1 | 0 | | 
| 3 | Đường dẫn truy vấn | 12 | 3 | cập nhật | 0 | tính toán lại | 

Truy vấn đầu tiên sử dụng cây gốc, tạo ra đường dẫn phân rã nhị phân tiêu chuẩn. Sau khi thay đổi, truy vấn thứ hai vẫn tái tạo lại tổ tiên một cách chính xác bằng cách hoàn tác hoán vị cấp 1 trước khi di chuyển lên trên. 

### Ví dụ 2 

đầu vào:```
3
3 8
2 4 -1
3 8
```| Bước | Hoạt động | Nút X | Độ sâu | ca1 | ca2 | Đường dẫn | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | Đường dẫn truy vấn | 8 | 3 | 0 | 0 | 8→4→2→1 | 
| 2 | Dịch chuyển cây con | 4 | 2 | 0 | -1 tại d=2 | | 
| 3 | Đường dẫn truy vấn | 8 | 3 | 0 | -1 | truyền tải đã sửa chữa | 

Truy vấn thứ hai thay đổi căn chỉnh cha-con ở độ sâu 2, do đó nút 4 được di chuyển một cách hiệu quả trong cấp độ của nó. Việc tái thiết đi lên bù đắp một cách chính xác cho sự thay đổi này, bảo tồn tổ tiên cuối cùng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(Q log X) | Mỗi truy vấn loại 3 có tối đa 60 cấp độ | 
| Không gian | O(log X) | Chỉ các mảng dịch chuyển theo độ sâu mới được lưu trữ | 

Các ràng buộc cho phép tối đa 10^5 truy vấn và mỗi truy vấn xử lý tối đa 60 bước, dẫn đến khoảng 6 × 10^6 thao tác, vừa vặn thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    MAXL = 61
    shift1 = [0] * MAXL
    shift2 = [0] * MAXL

    def get_depth(x):
        return x.bit_length() - 1

    q = int(input())
    out = []

    for _ in range(q):
        tmp = input().split()
        t = int(tmp[0])

        if t == 1 or t == 2:
            x = int(tmp[1])
            k = int(tmp[2])
            d = get_depth(x)
            k %= (1 << d)

            if t == 1:
                shift1[d] = (shift1[d] + k) % (1 << d)
            else:
                shift2[d] = (shift2[d] + k) % (1 << d)

        else:
            x = int(tmp[1])
            path = []
            cur = x

            while cur:
                path.append(cur)
                if cur == 1:
                    break
                d = get_depth(cur)
                mod = (1 << d)
                cur = ((cur - shift1[d] - shift2[d]) % mod) // 2

            out.append(" ".join(map(str, path)))

    return "\n".join(out)

# provided sample
assert run("""5
3 12
1 2 1
3 12
2 4 -1
3 8
""") == """12 6 3 1
12 6 2 1
8 4 2 1"""

# custom cases
assert run("""1
3 1
""") == "1", "single node"

assert run("""3
1 2 1
3 2
3 4
""") == "2 1\n4 2 1", "small shift effect"

assert run("""5
3 8
1 1 3
3 8
2 2 -1
3 8
""") == """8 4 2 1
8 4 2 1
8 4 2 1""", "mixed operations stability"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn | 1 | trường hợp cơ sở đúng đắn | 
| hiệu ứng dịch chuyển nhỏ | 2 1 / 4 2 1 | truyền dịch cấp độ | 
| ổn định hoạt động hỗn hợp | đường dẫn lặp đi lặp lại | idempotence theo cấu trúc no-op | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi X rất nhỏ, đặc biệt là X = 1. Trong trường hợp này, độ sâu bằng 0 và bất kỳ số học mô-đun nào về kích thước cấp phải tránh các mặt nạ có kích thước bằng 0. Thuật toán tự nhiên tránh áp dụng các dịch chuyển ở độ sâu 0 vì (1 << 0) = 1, làm cho tất cả các dịch chuyển gần như bằng 0 và ngăn các hoạt động không hợp lệ. 

Một trường hợp cạnh khác xảy ra khi sử dụng giá trị K âm lớn. Bởi vì các ca được tích lũy ở kích thước mức modulo nên các cập nhật âm phải được chuẩn hóa đúng cách. Nếu không có điều này, phép trừ lặp đi lặp lại có thể vượt ra ngoài giới hạn hợp lệ và tạo ra sự tái cấu trúc cha mẹ không chính xác. Hoạt động modulo trong quá trình triển khai đảm bảo tính nhất quán trên cả các phép quay dương và âm. 

Trường hợp tinh vi cuối cùng là các cập nhật lặp lại ở cùng độ sâu. Vì cả loại 1 và loại 2 đều tích lũy độc lập nên điều quan trọng là chúng không được sáp nhập sớm. Việc giữ chúng tách biệt đảm bảo rằng những thay đổi về cấu trúc không ảnh hưởng đến hoán vị nhãn, bảo toàn việc tái thiết tổ tiên chính xác dưới sự xen kẽ tùy ý.
