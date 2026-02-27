// ==========================================
// PART 1: SHARED TYPES & MODELS (types.ts)
// ==========================================

export enum TerrainType {
  FLAT = "FLAT",
  HILL = "HILL",         
  MOUNTAIN = "MOUNTAIN", 
  CHASM = "CHASM",
  DROPPED_SHARD = "DROPPED_SHARD"
}

export enum AbilityType {
  REACH = "REACH",           
  CLEAVE = "CLEAVE",         
  TERRAFORM = "TERRAFORM",   
  VOLLEY = "VOLLEY",
  NONE = "NONE"
}

export interface Unit {
  id: string;            
  ownerId: string;       
  type: string;          
  basePower: number;
  currentHealth: number;
  maxMovement: number;
  ability: AbilityType;
  hasActivatedThisTurn: boolean;
}

export interface Tile {
  x: number;
  y: number;
  elevation: number;     
  terrain: TerrainType;
  occupant: Unit | null; 
}

export enum SocketEvents {
  JOIN_MATCH = "JOIN_MATCH",
  MATCH_FOUND = "MATCH_FOUND",
  STATE_UPDATE = "STATE_UPDATE",   
  ACTION_MOVE = "ACTION_MOVE",     
  ACTION_ATTACK = "ACTION_ATTACK", 
  ACTION_BUY = "ACTION_BUY",       
  PHASE_CHANGE = "PHASE_CHANGE"    
}

// ==========================================
// PART 2: CORE GAME ENGINE (engine.ts)
// ==========================================

export class BoardState {
  public grid: Tile[][];
  public width: number;
  public height: number;

  constructor(width: number, height: number) {
    this.width = width;
    this.height = height;
    this.grid = this.generateEmptyGrid(width, height);
  }

  private generateEmptyGrid(width: number, height: number): Tile[][] {
    // AI Prompt target: "Write a function to populate a 2D array of Tiles"
    return [];
  }

  public getTile(x: number, y: number): Tile | undefined {
    // AI Prompt target: "Return the tile if within bounds, handle out-of-bounds"
    return undefined;
  }

  public moveUnit(fromX: number, fromY: number, toX: number, toY: number): boolean {
    // AI Prompt target: "Verify unit exists, target is empty/passable, update occupants"
    return true;
  }
}

export class PathfindingEngine {
  private board: BoardState;

  constructor(board: BoardState) {
    this.board = board;
  }

  public getValidMoves(startX: number, startY: number, maxMovement: number): Tile[] {
    // AI Prompt target: "Implement a BFS algorithm to find all reachable tiles"
    return [];
  }
  
  public checkLineOfSight(fromX: number, fromY: number, targetX: number, targetY: number): boolean {
    // AI Prompt target: "Draw a line between coordinates, return false if it hits a MOUNTAIN"
    return true;
  }
}

export class CombatEngine {
  private board: BoardState; 

  constructor(board: BoardState) {
    this.board = board;
  }

  public executeAttack(attackerX: number, attackerY: number, targetX: number, targetY: number) {
    const attacker = this.board.getTile(attackerX, attackerY)?.occupant;
    const defender = this.board.getTile(targetX, targetY)?.occupant;
    if (!attacker || !defender) throw new Error("Invalid combat targets");

    const flankCount = this.calculateFlanking(targetX, targetY, attacker.ownerId);
    
    // AI Prompt target: "Calculate Net Damage: Math.max(0, (attacker.basePower * (1 + 0.5 * flankCount)) - defenderArmor)"
    
    if (defender.currentHealth <= 0) {
      this.handleUnitDeath(targetX, targetY);
    }
  }

  private calculateFlanking(targetX: number, targetY: number, attackerOwnerId: string): number {
    // AI Prompt target: "Check orthogonal adjacent tiles, count matching attackerOwnerId units"
    return 0;
  }

  private handleUnitDeath(x: number, y: number) {
    // AI Prompt target: "If deadUnit is 'Shardbearer', set terrain to DROPPED_SHARD. Set occupant to null."
  }
}

export class AbilityManager {
  public triggerAbility(unit: Unit, startX: number, startY: number, targetX: number, targetY: number) {
    // AI Prompt target: "Create switch statement handling AbilityType (e.g. CLEAVE AoE logic)"
  }
}

// ==========================================
// PART 3: THE STORMLIGHT ECONOMY (market.ts)
// ==========================================

export class PlayerEconomy {
  public playerId: string;
  public currentStormlight: number = 0;
  public baseIncomePerRound: number = 100;

  constructor(id: string) {
    this.playerId = id;
  }

  public applyRoundIncome() {
    // AI Prompt target: "Add baseIncome, cap at maximum treasury limit"
  }

  public spend(amount: number): boolean {
    if (this.currentStormlight >= amount) {
      this.currentStormlight -= amount;
      return true;
    }
    return false;
  }
}

export interface MarketAsset {
  unitType: string;
  baseFirmCost: number;     
  currentPrice: number;     
  demandVolatility: number; 
  unitsBoughtThisRound: number;
}

export class MarketEngine {
  public assets: Record<string, MarketAsset> = {
    "Spearman": { unitType: "Spearman", baseFirmCost: 20, currentPrice: 20, demandVolatility: 1.1, unitsBoughtThisRound: 0 },
    "Cavalry": { unitType: "Cavalry", baseFirmCost: 50, currentPrice: 50, demandVolatility: 1.25, unitsBoughtThisRound: 0 },
    "Shardbearer": { unitType: "Shardbearer", baseFirmCost: 150, currentPrice: 150, demandVolatility: 1.5, unitsBoughtThisRound: 0 }
  };

  public executeTrade(player: PlayerEconomy, unitType: string): Unit | null {
    // AI Prompt target: "Deduct currentPrice from player, increment unitsBought, recalculate price"
    return null;
  }

  public stabilizeMarket() {
    // AI Prompt target: "Reduce currentPrice by 10% if unitsBoughtThisRound is 0, reset counters"
  }
}

export class DraftManager {
  private playerReserves: Record<string, Unit[]> = {}; 

  public deployUnit(playerId: string, unitId: string, targetX: number, targetY: number): boolean {
    // AI Prompt target: "Verify target is in valid starting rows, tile is empty, remove from reserves"
    return true;
  }
}

// ==========================================
// PART 4: MULTIPLAYER SERVER (server.ts)
// ==========================================

export class TurnManager {
  private turnTimer: NodeJS.Timeout | null = null;
  private readonly TURN_DURATION_MS = 30000; 

  public startTurnTimer() {
    // AI Prompt target: "Set timeout to advance phase and broadcast PHASE_CHANGE"
  }
}

export class GameRoom {
  public roomId: string;
  public board: BoardState;    
  public market: MarketEngine; 
  public currentPhase: "DRAFT" | "P1_TURN" | "P2_TURN" | "RESOLUTION" = "DRAFT";
  public roundCount: number = 1;

  constructor(roomId: string) {
    this.roomId = roomId;
    this.board = new BoardState(10, 10);
    this.market = new MarketEngine();
  }

  public processMoveRequest(playerId: string, fromX: number, fromY: number, toX: number, toY: number) {
    // AI Prompt target: "Verify turn, validate move via PathfindingEngine, update board, broadcast state"
  }

  public triggerHighstorm() {
    // AI Prompt target: "Iterate grid. If occupant is not on HILL/MOUNTAIN, apply damage"
  }
}

// AI Prompt target: "Write Socket.io listener mapping SocketEvents to GameRoom methods"

// ==========================================
// PART 5: CLIENT RENDERING (client.ts)
// ==========================================

export const GameTheme = {
  factions: {
    playerOne: { primaryColor: "#0033A0", glowEffect: "rgba(0, 51, 160, 0.5)" },
    playerTwo: { primaryColor: "#8B0000", glowEffect: "rgba(139, 0, 0, 0.5)" }
  },
  ui: {
    highlightMove: "rgba(255, 255, 255, 0.3)", 
    highlightAttack: "rgba(255, 0, 0, 0.4)"    
  }
};

export class IsometricCamera {
  public tileWidth: number = 64;  
  public tileHeight: number = 32; 
  public elevationScale: number = 16; 

  public gridToScreen(gridX: number, gridY: number, elevation: number) {
    const screenX = (gridX - gridY) * (this.tileWidth / 2);
    let screenY = (gridX + gridY) * (this.tileHeight / 2);
    screenY -= (elevation * this.elevationScale);
    return { x: screenX, y: screenY };
  }
}

export class GameRenderer {
  private ctx: CanvasRenderingContext2D;
  private camera: IsometricCamera = new IsometricCamera();

  constructor(canvasId: string) {
    const canvas = document.getElementById(canvasId) as HTMLCanvasElement;
    this.ctx = canvas.getContext("2d")!;
  }

  public renderFrame(board: BoardState) {
    // AI Prompt target: "Clear screen, loop through board.grid, call renderIsometricTile"
  }

  private renderIsometricTile(screenX: number, screenY: number, terrain: TerrainType) {
    // AI Prompt target: "Draw an isometric polygon using canvas ctx based on TerrainType"
  }
}

export class UIManager {
  public showUnitStats(unit: Unit) {
    // AI Prompt target: "Generate HTML/CSS overlay for Unit stats"
  }
}
