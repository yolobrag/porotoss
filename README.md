using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using LeagueSharp;
using LeagueSharp.Common;
using SharpDX;
using Color = System.Drawing;

namespace PoroShooter
{
    class Program
    {
        public static Spell PoroThrow;

        static void Main(string[] args)
        {


            Game.OnGameUpdate += Game_OnGameUpdate;
            CustomEvents.Game.OnGameLoad += Game_OnGameLoad;
        }


        private static void Game_OnGameLoad(EventArgs args)
        {
            var spell = ObjectManager.Player.GetSpellSlot("summonerporothrow");
            if (spell == SpellSlot.Unknown)
            {
                Console.WriteLine("RETURN");
                return;
            }
            var slot = ObjectManager.Player.Spellbook.GetSpell(spell);

            PoroThrow = new Spell(spell, 2500f);
            PoroThrow.SetSkillshot(.5f, 50f, 1100, true, SkillshotType.SkillshotLine);

        }

        private static void Game_OnGameUpdate(EventArgs args)
        {
            if (PoroThrow.Instance.State == SpellState.Ready){
                Console.WriteLine("UPDATE");
                foreach (var champ in ObjectManager.Get<Obj_AI_Hero>().Where(h => h.IsValidTarget(PoroThrow.Range)))
                {
                    PoroThrow.Cast(champ);

                }
            }
        }

    }
}
